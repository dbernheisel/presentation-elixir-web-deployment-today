footer: @bernheisel --- WHY ELIXIR?
theme: Titillium (Better Code)
slidenumbers: true

# [fit] Why I
# [fit] Enjoy
# [fit] Elixir

---

# [fit] developer
# [fit] perspective

---

# [fit] Monolith

^ First we started with monoliths. but then we got tired of how
difficult it was for objects to not mess with each other.

---

Objects managing their own state

![autoplay mute](./darkest-timeline.mp4)

---

# [fit] Microservices

^ Then we replaced with microservices so outages could feel more like a
murder mystery and our devops friends could get paid more

---

![autoplay mute](./mexican-standoff.mp4)

---

# [fit] Maintainable
# [fit] Extendable
# [fit] Enjoyable

^ I want manageable applications in any language, and I have found that
easiest to with Elixir. Honestly don't care if that means a lot of small
applications, a set of medium applications, or a large application.

---

# What makes software enjoyable to work on?

* Tooling
* Debugging
* Readability
* Limited WTF moments
* Micropatterns

[.build-lists: true]

---

![autoplay mute](./too-clever.mp4)

# !CLEVER

^ If I review someone else's code and see a clever method, I usually
find it because I had to read that code several times to understand it.

^ It's not because I'm dumb; it's not because they're a genius; it's
because they used an edge-case in the language to accomplish their goal
at the cost of revealing their intention. It's not as readable. It's not
like reading a story.

^ Clever code is not saving anyone time. It's wasting mine, and probably
yours, and definitely your future self's when you realize you can read
whatever bullshit you just wrote.

---

# [fit] UNDERSTOOD

^ I want to be the most understood person in the room. I want anyone
that looks at my code to be able to understand it and extend it.

---

# [fit] ELIXIR

^ So, let's talk about Elixir. Elixir is a language introduced about 7
years ago in 2011. It's a higher-level language that tricks you into
thinking you're writing Ruby or Python (moreso Ruby). It was developed
by a person named Jose Valim who was big in the Ruby community and was
struggling to make Ruby work well with concurrency. He decided o instead
leverage Erlang's platform, but found writing Erlang too obtuse.
Introducing Elixir.

^ It compiles down to Erlang bytecode. What is Erlang?

^ Erlang is a 32 year old language originally developed for making the
telephone systems stable and without downtime. The BEAM Virtual Machine
compiles it to C and is extremely fast, and is coupled with a convention
of process.

---

## Micropatterns

* Pipelines
* Composition
* Totality
* Monads
* Happy path

---

### Pipelines

```elixir
def load(filename)
  filename
  |> read()
  |> parse()
  |> validate()
end
```

^ Pipelines present a clear path of input and output, and what is
happening at every train stop.

---

[.background-color: #FFFFFF]

railway examples

^ The big thing here is that everything is very predictable. You always
have an input, and you always get output.

---

Data moving through transformations

![autoplay mute](./information-highway.mp4)

---

### Pipelines

```elixir
def load(filename)
  filename
  |> read()
  |> parse()
  |> validate()
end
```

^ It's a system of functions working together, and we stitch these
functions together to make longer pipes of data, and these pipes
constructed into other pipes, and so on

---

### Totality

```elixir
@spec load(String.t) :: [Row.t]
def load(filename), do: [%Row{id: 1}, ...]
```

```elixir
@spec read(String.t) :: Enumerable.t
def read(filename) do
  case filename do
    nil ->
      raise FileError, "Filename not supplied"
    filename ->
      filename |> read_file!
  end
end
```


^ The first function is simple, and total.

^ The second function is not total because it has the option to raise an
exception. Please don't do this, it's awful and performance-wise is
never better. As a developer, I don't need you to decide for me to raise
an exception in my application, I need you to be a monad.

^ Defining types in Elixir is optional, but when you have it, it serves
as documentation to other developers, and it indicates what kind of
input and output you should expect.

^ There is tooling available to check if you're honoring your typespecs
at compile-time

---

### Monad

```elixir
@spec read(String.t) :: {:ok | :error, Enumerable.t}
def read(filename) do
  case filename do
    nil ->
      {:error, 'Filename not supplied'}
    filename ->
      {:ok, read_file!(filename)}
    end
  end
end
```

^ This first function is a monad. All it's doing is giving you metadata
about the function's result. In this case, I'm returning a tuple that
allows me to switch upon it in the pipeline. The first element in the
tuple is either an :ok atom, or an :error atom.

---

Show railway images with branches

---

### Happy Path

```elixir
@spec parse({:ok | :error, Enumerable.t}) :: {:ok | :error, Enumerable.t}
def parse({:ok, raw_rows}) do   # Happy Path :)
  parsed_rows =
    Enum.map(raw_rows, fn raw_row ->
      %Row{
        name: raw_row[0],
        email: raw_row[1],
      }
    end
  {:ok, parsed_rows}
end
def parse(errors), do: errors   # Unhappy Path :(
```

^ It's easy to program for the happy path when the language includes
pattern-matching. With pattern-matching, I can easily identify what I
need from the input so I can transform the data and return it as the
output.

^ It's also easy to recognize that I have limited my happy path in the
clause, and it's equally as easy to include an unhappy path clause right
after it. In this case, _only_ in the case of an {:ok} tuple will it
proceed to transform; otherwise, the other clause will match and simply
pass on the data as-is with no transformation.

^ Programming for the unhappy path is more difficult in other languages
without pattern-matching. It's more difficult because it's easier to
forget about Nil

---

# [fit] Nil
# [fit] NoMethodError

---

### Composition

```elixir
@spec read(String.t) :: {:ok | :error, Enumerable.t}
def read(filename) do
  case filename do
    nil ->
      {:error, 'Filename not supplied'}
    filename ->
      {:ok, read_file!(filename)}
    end
  end
end
```

---

### Composition

```elixir
@spec read_file!(String.t) :: Enumerable.t
def read_file!(filename) do
  filename
  |> File.stream!
  |> CSV.decode!
end
```

---

### Composition

> OOP has objects in the large, and methods in the small
> FP has functions in the large, and functions in the small

-- Scott Wlaschin (Functional Programming Design Patterns)

---

### Composition

```elixir
response =
  request
  |> router()
  |> controller()
  |> view()
```

---

### Functions all the way down

![autoplay loop mute](./turtles-all-the-way-down.mp4)

---

^ I've been priming you for the big reveal.

---

# [fit] NOT
# [fit] MICRO

---

> That's how Plug works
> That's how Phoenix works
> That's how Ecto works
> That's how Absinthe works
> That's how Hex works
> That's how everything works

---

---
| Object Oriented       | Functional         |
|-----------------------|--------------------|
| Single Responsibility | functions          |
| Open/Closed           | fn                 |
| Liskov substitution   | functions, also    |
| Interface Segregation | functions, still   |
| Dependency Inversion  | objects...jk. fn   |
| Factory pattern       | still functions    |
| Decorator pattern     | fun                |
| Visitor pattern       | FUNCTIONS          |

---

# [fit] Elixir has
# [fit] excellent
# [fit] patterns

---

# [fit] Maintainable
# [fit] Extendable
# [fit] Elixir

---

# [fit] @bernheisel
# [fit] Viget Labs

Sources:
[Cameron Price: Micropatterns](https://www.youtube.com/watch?v=9uvp4h7gXHg)
[Scott Wlaschin: Functional Programming Design Patterns](https://www.youtube.com/watch?v=srQt1NAHYC0)
