----
title: Developing Fun
author: David Bernheisel
date: 2019-02-25
----

Have you ever read code you _hate?_.
Have you ever read code you _love?_.

We're not talking about _wrong_ code or misunderstood requirements; we're talking about 2 different solutions to the same problem; code that accomplishes the job -- you just don't like it.

I do this all the time; usually the first time I write some code, I really don't like it. It accomplishes the task, but then I refactor it into something I do enjoy through the lessons I just learned, and especially after PR review.

That let me to wonder, "Why do we enjoy some software over other software?" There are some codebases that I come to that I just _hate_, and then there are others that are _lovely_. What makes them different?

A lot of times, we point to languages, frameworks, or the code's testability, for example is it easy to get instant feedback on your change? If you know about Ruby on Rails, then you know that generally when it released, it made MVC architecture really enjoyable compared to what existed before, even outside of Ruby. It made development really enjoyable for a lot of folks. Why?

A typical discussion path from here leads to comparing languages and frameworks as our source of enjoyable software, feature sets vs feature sets; but that would be short-sighted. While frameworks and languages certainly can contribute to enjoyable software, they don't define it. A framework can have a limited feature-set, but _do it really well and be loved_.

Before jumping into the technicals, I'd like to explore this more psychologically what makes development happy and sad, zealous and angry, smart and dumb. So, let's step outside of the realm of software and explore a concept that everyone generally agrees is enjoyable.

---

# Music

Everyone listens to music and when you do, you enjoy it! There's no denying it; sure, there are music styles you don't like, but we're not talking about that; we're talking about the music _you_ like. Close your eyes and hear it now. Classical, rock, hip-hop, pop, or Mongolian throat singing. You can hear it play in your mind; you hum the tune sometimes mindlessly; when doing dishes or laundry, you might beatbox the beat. Music gets us in tune.

[Mario theme start]

Music can imprint a vision in your mind. You're probably imagining something from Super Mario Bros, right?

[Mario image]

Maybe not this _exact_ image, but close! Music can transport us into a different time, age, and space.

[Matchbox 20 intro]

This takes me back to my mom's room. I have a small Magnavox boombox sitting on the desk, and I'm playing a game, Final Fantasy 7, on her computer. This was 22 years ago and I remember it vividly, and yet I don't remember the day I got my driver's license.

[Brain image]

When we hear music, areas of our brain start to light up. When we focus on specific parts of the song like the lyrics, or the instrumentation, then different areas of the brain activate. We listen to the song on repeat to soak all that dopamine in; it becomes more memorable.

Combine all that with an emotional situation outside of the song, like falling in love with someone, a hard breakup, a death, a fantastic camping trip, music festival, your own wedding, or maybe just a peaceful walk downtown, and you have the recipe for a song that transports you to a different time.

This symphony of chemicals in your brain calls this a memory. They remind of something we know deeply.

Why does music light us up? To find our answer, let's talk about design.

# Video Games

Specifically, let's talk about Video Game design. Games are designed to be fun, so surely video game developers and designers know how to create fun, right? We don't have a scientific method for proving the theory of fun, so here on out is it's a little more pseudo-science, but take a chance with me.

There's a book by a fellow named Raph Koster named "A Theory of Fun", I highly encourage you to find his talks and books and listen. It's highly insightful for us today.

"When we see a pattern, we delight in tracing it, and seeing it reoccur" - Raph Koster

"Fun is the emotional response to learning" - Chris Crawford

"Edutainment"

Let's look at some famous examples:

[Super Mario Bros - Level One]

These games teach us patterns, and then delightfully introduce variations on them.

In the original Super Mario Bros, we know that Mario moves right to advance to the goal; we can tell because he's on the left side of the screen, and you can't go left; you must go right.

- It introduces a pattern by establishing if you touch the Goombas, you die. Noted-- don't touch bad guys.
- Turns out Mario can jump! Let's avoid those bad guys. Noted-- I can move right and jump
- It introduces a pattern by establishing if you touch the mushroom, you grow. Noted-- there are power-ups that make me stronger
- You experiment with two elements: Getting a mushroom, and then touching the enemy. Turns out that's a mistake and now you shrink back down. Noted-- can't kill bad guys by only being big.
- You experiment with two more elemnts: jump on the enemy's heads!. Noted-- bad guys can be jumped on.

These are universal patterns now in games. We know instinctively that we just don't touch bad things.

Musically, we're getting feedback:

[Mario getting a mushroom]
[Link opening a chest]
[Final Fantasy victory fanfare]

It's a nice and tight communication cycle. We open chest, we get reward. We beat bad guys, we get points.

Thinking back to pop music, almost every song on the radio has a recognizable pattern, whether you've picked it out or not: Intro, Verse, Chorus, Verse, Chorus, Bridge, Chorus, Outro.

If we analyzed the notes themselves, there are patterns of notes that musicians know sound good together called scales. Notes played that are not on the given scale will create a harsh sound creating dissonance in the sound waves; an out-of-tune guitar, for example.

Video game designers know this as well and know that they really can't have lyrics being blasted at you while you're reading text on the screen and moving your characters around. Have you ever turned down the radio when you're getting close to your destination? We do this because we're trying to focus.

Likewise, video games want to focus your attention on the game, not the lyrics or even the music by itself; they want to envelope you into the world.

So, what is fun? In graph form, it looks a little like this:

+----------------------------------------------------------+
|                                                          |
|              All forms of feedback:                      |
|     Art, animation, sound, music, movement, story.       |
|                                                          |
+-------+-------------------------------------------^------+
        |                                           |
        |                 A GAME ATOM               |
        | updates                                   |
        |                                      +----+-----------+
        |                                      |                |
   +----v--------+    +---------------+        |     place      |
   |             |    |               | input  |                |
   |   problem   +---->  preparation  +------->+ +------------+ |
   |             |    |               |        | |    core    | |
   +-------------+    +---------------+        | |  mechanic  | |
                                               | +------------+ |
                                               |                |
                                               +----------------+

Wow! This looks familiar.

This looks like TDD!

Let's change the words a little bit:

+----------------------------------------------------------+
|                                                          |
|              All forms of feedback:                      |
|      code review, performance, test suite, correctness,  |
|               those dang customers                       |
+-------+-------------------------------------------^------+
        |                                           |
        |                 A SOFTWARE UNIT           |
        | updates                                   |
        |                                      +----+-----------+
        |                                      |                |
   +----v--------+    +---------------+        |      test      |
   |             |    |               | spike  |                |
   |   problem   +---->  preparation  +------->+ +------------+ |
   |             |    |               |        | |   logic    | |
   +-------------+    +---------------+        | |            | |
                                               | +------------+ |
                                               |                |
                                               +----------------+

- We're presented a problem,
- We prepare for a solution
- Write a test (place)
- Run the logic (core mechanic)
- And adjust according to the feedback we experience.


It turns out that developers are players in the game of software. Do you know what this means?

YES WE DID IT
WE'RE IN TRON

[Tron image]

[Mission Accomplished meme]

We're full circle now.
So, the journey so far:

- We are developers that solve problems for a living.
- Sometimes these problems aren't fun.
- We now know that fun is: learning recognizable and reoccurring patterns with quick feedback loops.
- We want development to be fun.

---

# Development

Now, let's apply this to development. So, now we know it's all about patterns, what patterns are worth repeating?

Let's talk about JavaScript.

[Image of blue]

In the beginning, there was a vast ocean with no known islands. It was a frustrating time. JavaScript pirates were sailing the high seas, plundering and collecting all the knowledge they could find. The only pattern was that there were no patterns. Every dev for devself, equipped with their cannons: copy and paste.

[Image blurred islands]

The JavaScript pirates started to settle onto a newly discovered island and organize themselves into a government. We'll call this island jQuery. Everyone rejoiced; it seems that pirates no longer needed to plunder, the JavaScripters can plant and plow like civilized people. More time passes, and more islands are discovered.

Some adventurous javascripters, having grown discontent plowing and planting, started to move to these islands and develop their own culture. These cultures grew and grew and grew. We'll call these islands Emberton, React Town, Vueville, Angular City.

[Image islands]

This may seem obvious now, but early days of JavaScript programming were hard, frustrating, and thoroughly unenjoyable. The opposite of fun. However, with the rise of recognizable patterns, like Node, Vue, and *cough* Webpack, it has made the world a better place. Developers can now communicate much more effectively and have smaller feedback loops.

---

# What patterns are worth repeating?

From here on out, I'm going to use my favorite language Elixir for examples. If you don't know Elixir, that's OK. I can prep you:
It's a high-level functional programming language that reads like Ruby, compiles to the battle-tested platform of Erlang, can run on a Raspberry Pi, power the entire world's telephone system, push OpenGL graphics, and serve up to 2 million websockets concurrently on a single Digital Ocean instance.

If the language you're using doesn't let you easily perform these patterns, that's ok. There are other good patterns in your language of choice. Find them. Or learn Elixir, that's great too.

# Pipelines

# Totality (Typespecs)

# Monads (Maybe, Tuple Metadata, Accumulators)

Not to say that Elixir has complete support for academic and mathematical monads, but the community's code style for Elixir leans towards using monads.

If you don't know what a monad is, that's totally fine. The simplest definition: A monad is a unit of data and metadata about the data.

The best and smallest example I can think of for monads is the Maybe Monad:

```elixir
case my_function() do
  {:ok, success_data} -> happy_path()
  {:error, data_about_error} -> unhappy_path()
end
```

Another example:

```elixir
reducer = fn (data, accumulator) ->
  case my_function() do
    {:ok, data} -> {:cont, data}
    {:error, data} -> {:halt, data}
  end
end

final_fata = Enum.reduce_while(stream, &reducer)
```


# Happy-path ()

# Composition (Functions in the small and large)




