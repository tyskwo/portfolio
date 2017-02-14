---
title: "Empire: The Need to Explore"
description: "Empire: the final project for AI. The first step is to explore the map, but what's the most efficient way to do this?"
---

_Empire:_ the final project for AI this semester, and a game that gave birth to the likes of _Civilization_ and _Age of Empires_. Because this game is more complex than _Minesweeper_ and _Rummy_, the class is going to tackle the project in stages, the first of which is exploring as much of the map as possible in a set amount of turns.

<figure class="align-left" style="width: 125px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/11/exploration_reasoning.png">
  <figcaption>The light green tiles are the ones that would be revealed when the red unit moves.</figcaption>
</figure>

_Empire_ consists of cities and units. Cities are baked into the map, are either owned by a player or neutral, and when attacked, can switch hands between players. They produce units, and the rate is determined by what type of unit. Unit types are armies, which are your basic one health, one movement type (they are also the only land-based units), cruisers, which can move the farthest in water, destroyers, which have a ton of firing power, and battleships, which are the kings of the sea.

Although armies are the only land-based unity, they can also travel over water (I imagine them piling into a little canoe). Every turn, there is a small chance of a storm occurring, which would wipe out the armies that are at sea. Thus, there is an inherent risk to sending armies to explore the sea with no coherent point to move towards.

<figure class="align-left" style="width: 300px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/11/explanation.png">
  <figcaption>These lines represent the exploration of every tile, using only 20 units over 45 turns (assuming we only produce armies).</figcaption>
</figure>

Regardless, from a pure exploration standpoint with no repercussions of enemies or having lone scouts, there's a smart way to look at this problem from a pure statistical lens. When moving in the four cardinal directions, units only expose three new tiles, whereas moving along the diagonals reveal five new tiles. Thus, my goal for this exploration challenge is to move all of my units on diagonals. I'll find the percentage of water:land tiles neighboring my starting city, and figure out how often I need to create a water-based unit. I will then have an offset to push them in parallel diagonals. Doing this in all four diagonals, weighting the one towards the center of the map, will statistically allow me to explore the largest amount of tiles. Areas of refinement include finding the right ratio of armies to water-based units and how best to deal with armies being lost at sea. That's my plan of attack, and it doesn't make much sense from a gameplay point-of-view, but for this assignment I believe it's the optimal strategy.
