---
title: "Empire: Time to Conquer"
description: "The land has been surveyed, cities found, and enemies identified. It is now time to claim reign of the land and declare victory!"
---

The land has been surveyed, cities found, and enemies identified. It is now time to claim reign of the land and declare victory! Defeating enemies, taking control of cities, and building the largest army are now all valid goals. Building off the last post of exploration, it's now time to create a plan of attack to, well, attack.

<figure class="align-center" style="width: 300">
 <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/12/empire.png">
  <figcaption>I opened up the entire map here, but in-game there is per-player fog of war. This still allows my exploration AI from last time to be relevant and stay in production.</figcaption>
</figure>

[Influence maps](http://www.redblobgames.com/x/1510-influence-maps/) are a form of world mapping to give information to AI opponents. They are similar to [blackboards](http://aigamedev.com/open/article/static-blackboard/), but give a more structured view of the world. Generally, blackboards are a shared resource among individual units, each with their own brain, whereas influence maps are more of a top-down approach, all of the units follow the influence map's reasoning. Also, influence maps can be weighted and blended: for instance, if we have maps for enemy density, city density, and ally density, we can combine these to have a master influence map. The efficiency of influence maps lie within the algorithm and reasoning for creating values at each tile, but having only one algorithm per map is much easier to manage than a single, larger complicated algorithm per unit for the blackboard. With Empire being tile-based, influence maps are [a breeze to implement.](http://aigamedev.com/open/tutorial/influence-map-mechanics/)

What's important to remember is that influence maps are relatively cheap, so long as their algorithms for determining the value of each tile are as well. Keeping this in mind, we could implement different influence map sets for different types of units: scouts can focus on unexplored land, whereas armies can focus on taking over neutral cities, and patrolmen can focus on defending their cities from enemies. This would easily allow for creation of new categories of units so long we can conceptualize an appropriate map for it. Influence maps are easy to implement will maintaining an extremely high level of customization. They aren't always the best choice, but with a tile- and turn-based strategy game like _Empire_, it's almost a no brainer.

<figure class="align-center">
 <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/12/influencemap.png">
  <figcaption>Here's a visual representation of an influence map, based on player control of a space.</figcaption>
</figure>
