---
title: Blackjack Simulation
date: 2021-10-24 20:43:00 -0300
categories: [Blog]
tags: [R, simulation, shiny, ggplot]
---

**Tools used:** R, ggplot, Shiny

**Category:** Simulation

---

<!--more-->

I got inspired to simulate a blackjack game, so I decided to make a series of functions to emulate the dealer's behavior, a newbie player, a cautious player, and a strategist. With this set of functions, you can run a single game with **p** players, **d** decks, any combination of players archetypes. And you can also run it **n** times.

I also made a [shiny app](https://jaqueline-medeiros.shinyapps.io/appa/) to display how the simulation works. In the app, you are limited by the number of players, but if you wish to run the code with more players you can check the GitHub [repository](https://github.com/devmedeiros/blackjack-simulation). There you can find the rules considered for the simulation and the complete code.

If you are familiar with the R language you can also run the app locally, you just need to run the `library(shiny)` and `runGitHub("blackjack-simulation", "devmedeiros", ref = "main")`.

The app is composed of a sidebar with a space to choose the archetypes, the number of decks to use, and how many rounds you want to see simulated. Depending on how many rounds you choose it can get slow as it'll simulate according to your choices every time you hit the **RUN SIMULATION RUN**.

In the plot tab, we have the evolution of the lose rate through the rounds.

A **1** means the player lost that round and a **0** means the player won.

![An image of the shiny app, plot tab](https://ik.imagekit.io/devmedeiros/plot_cm7Dhm0u6a.png?updatedAt=1635119435941)

The game setup tab shows all the cards dealt in the simulation, each card was dealt from left to right and a blank cell means the player didn't ask for another card (hit).

![An image of the shiny app, the game setup](https://ik.imagekit.io/devmedeiros/game-setup_-FspHIe5w.png?updatedAt=1635119436114)

And lastly, the lose rate tab shows us the same data from the plot tab, but now as a table. This is useful if you want to take a deeper look at how one strategy was better than the other.

![An image of the shiny app, the lose rate](https://ik.imagekit.io/devmedeiros/lose_rate_jPTu-cXHuN.png?updatedAt=1635119436123)
