# Why Elixir

## Micropatterns


```elixir
file
|> load()
|> parse()
|> validate()
|> do_stuff()
```

### Handling the happy path

Pattern-matching
"Let it crash"

"NoMethodError"
handling nil

### Transforming data vs Mutating state

Recursion in OO is usually a clever implementation, not easily
understood, so often avoided. Instead developers will reach for mutating
state.

## Maintainability

> OOP has objects in the large, and methods in the small
> FP has functions in the large, and functions in the small small
- Scott Wlaschin (Functional Programming Design Patterns)

How we handle validation

"But that's how Plug works"


## Making

- Mix
- Small scale applications, and scaling **in the same language** to a
    large scale application
-


