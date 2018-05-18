---
title: Deploying an Elixir Web App Today
theme: black
highlightTheme: atom-one-dark
separator: <!--s-->
verticalSeparator: <!--v-->
revealOptions:
  transition: none
  controls: true
# run `npm run watch` to start a local server
# s => speaker notes
# o => overview
---

# Deploying to Production Today

David Bernheisel @bernheisel

Viget Labs, LLC

Note: There are too many options for how to deploy an Elixir web app. With old
ways and new ways, old tools and new tools, and SaaS products that let
you ignore important features of the language, it's difficult for new
developers to write an app and feel good about how to introduce it to
the world.

Even though Elixir is relatively new to market, we face tool fatigue too
much already, so we're going to explore the popular options for
deploying Elixir web applications and boil the options down to two: (1)
easy, and (2) better.

<!--s-->

# Disclaimer
- Details may change in Elixir 1.7 <!-- .element: class="fragment" data-fragment-index="1" -->
- Some details may not matter depending on the strategy you choose <!-- .element: class="fragment" data-fragment-index="2" -->
- Nothing new in this talk <!-- .element: class="fragment" data-fragment-index="3" -->
- If you want something new, bitwalker <!-- .element: class="fragment" data-fragment-index="4" -->

<!--s-->

<!-- .slide: style="margin-top: -30%" data-background="./images/deployment_today/too-many-options.gif" -->

Note: Too many options

<!--s-->

## Too Many Options

|||
|-----|----|
| Containers? | Distillery  |
| Exrm        | Gatling     |
| Gigalixir   | Heroku      |
| Nanobox     | akd         |
| bootleg     | bottler     |
| eDeliver    | exdm        |
| exreleasy   | relx        |
| `mix run --no-halt` |     |

Note: There are too many options out there. This table shows the options
that I could easily find; mostly pulled from the Awesome Elixir list.
Please god let no project just ssh into a server and run mix run no
halt.

What this table reveals is a couple of problems, good problems to have,
but problems nonetheless.

<!--s-->

# Evident Problems

- Too many different ways to deploy
- High chance of tool fatigue
- It's been changing
- No wrongest or rightest way
- My goal is to boil it down to 2 options for _your_ project.

Note: These are great problems to have. There's no way that I could
discern in an evening which tool is best for my project. All of this at
once is paralyzing. Do you remember the first time you were learning
VIM, or emacs, or your GUI editor? It's exhausting.

The tools have been changing. Not all of these tools were eDeliver once
used exrm as its underlying release tool, then Distillery. New tools
have been released.

<!--s-->

# WHEN

<!--s-->

## Immediately

- Deployment option can change your code <!-- .element: class="fragment" data-fragment-index="1" -->
- Setting up the good practice <!-- .element: class="fragment" data-fragment-index="2" -->
- Lower your stress <!-- .element: class="fragment" data-fragment-index="3" -->

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/after-action-review.gif" -->
When a developer has to explain his/her salary after a bad deploy

<!--s-->

## Deployment Is Like Code

- The longer you wait, the more stressed you'll be
- The bigger the change, the more area surface area to debug
- Once established, you save your co-workers tool fatigue

Note: Make the deployment process established earlier, so it's no big
deal to deploy. Make it EASY, even if the process is hard.

<!--s-->

# How to Decide

### Size
1. Personal
1. Business <!-- .element: class="fragment" data-fragment-index="1" -->
1. Enterprise <!-- .element: class="fragment" data-fragment-index="2" -->

<!--s-->

# How to decide

### Resources
- Do you want to do it yourself?
- Do you have someone on your team that can do it? <!-- .element: class="fragment" data-fragment-index="1" -->
- Should you pay someone else to do it? <!-- .element: class="fragment" data-fragment-index="2" -->

Note: Not going to try to tell everyone what they should do; you know
your project the best. The difference is your interest and what
resources you have available.

This shouldn't come to anyone's surprise. It boils down to the value
prop of platforms out there.

<!--s-->

# Easy

<!--s-->

# SaaS

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/what-1.jpg" -->

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/what-2.gif" -->

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/what-3.gif" -->

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/what-4.jpg" -->

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/what-5.png" -->

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/what-6.gif" -->

<!--s-->

# Heroku | Gigalixir

<!--s-->

# Heroku

- Cheapest time cost
- Limited database connections
- Let's you ignore some compile-time vs run-time configuration
- Distributed clustering not available
- GenServers get reset
- Remote Observers/Shells not available.

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/no-like.png" -->

<!--s-->

# Gigalixir

- Cheapest time cost
- _**Severely**_ limited database connections (free)
- ~~Let's you ignore some compile-time vs run-time configuration~~
- ~~Distributed clustering not available~~
- ~~GenServers get reset~~
- ~~Remote Observers/Shells not available.~~

Note: But only one

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/like.gif" -->

<!--s-->

## Nuts and Bolts

- [if using Gigalixir] Add Distillery
- [if needed] Provision database
- Setup staging git remote
- Setup production git remote
- Let `git push production` and buildpacks handle it
- Write a simple `bin/deploy` script to handle the push and run
    migrations or tasks

<!--s-->

## `bin/deploy production`

```bash
#!/bin/sh
# Run this script to deploy the app to Heroku.

# Exit script if anything returns as an error
set -e

branch="$(git symbolic-ref HEAD --short)"
target="${1:-staging}"

git push "$target" "$branch:master"
heroku maintenance:on --remote "$target"

if [ "$target" != "staging" ]; then
  heroku pg:backups:capture --remote "$target"
fi
```

<!--s-->

## `bin/deploy production`

```bash
#...
heroku run "POOL_SIZE=2 mix ecto.migrate" \
  --exit-code --remote "$target"

if [ "$target" = "staging" ]; then
  heroku run "POOL_SIZE=2 mix development_seeds" \
    --exit-code --remote "$target"
fi

heroku restart --remote "$target"
heroku maintenance:off --remote "$target"
```

<!--s-->

https://hexdocs.pm/phoenix/heroku.html

<!--s-->

# Do not underestimate the value of DevOps

Note: There's a ton of other value props you get when using services
like Heroku. Load balancing; OS and database updates; reliability; DDOS
mitigation; marketplace; alerting

<!--s-->

<!-- .slide: style="margin-top: 25%" data-background="./images/deployment_today/swish.gif" -->

<!--s-->

# Better

<!--s-->

# Distillery*

Note: This is the part that could change with Elixir 1.7

<!--s-->

## Distillery + "Cloud"

- Complete control
- Maximize efficiencies
- Cheapest resource cost

Note: All the responsibility is yours

<!--s-->

## Nuts and bolts

- Setup project with Distillery.
- Setup a dockerfile for local compile.
- Spin up a DigitalOcean droplet, matching dockerfile environment
- Setup the server
  - Reverse proxy to the app
  - Create systemd autostart to start your app
- Setup the database
- Write a `bin/deploy` script to copy the release binary and restart the
    app

<!--s-->

## If you have even more time:
- Separate the database
- Git hooks
- Ansible script
- CDN Strategy
- Kubernetes for autoscaling
- Don't forget to backup the database

<!--s-->

<!--s-->

# Good problems

- People are experimenting
- Experimenting out loud gives everyone the chance to learn
- Elixir community is _large enough to give newbies fatigue_
- There will be a _common_ way soon, hopefully.

Note: I won't say that Elixir 1.7 introduces the _right_ way to deploy,
but only that it'll give mix the tools to that'll let the community
consolidate to a common pathway.

