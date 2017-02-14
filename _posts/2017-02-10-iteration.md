---
title: "Iteration."
description: "Just need to keep trucking along."
---

It's been a relatively slow week for <em>Office Mayhem</em>, but <em>9:5 Games</em> has still accomplished a lot this past week: the artists have been working on models, characters, and rigs, the designers have been producing new sound effects, building levels, and balancing systems, and the programmers have been finishing systems from last week. [Tony](http://tonyl.info) finished up the elevator from last week and implemented a revised dash system that uses a cool down to discourage players from constantly mashing the B-button as fast as humanly (or inhumanly) as possible. To combat this, the designers (and Tony) created a coffee pot item that allows players to move at their fastest speed for a little while after using it. It hasn't gone through much testing, but it has already created a couple new strategies from what we've seen.

[Andy](https://andrewmillsapblog.wordpress.com) spent this week finishing up crunch time to make it more pressure-inducing for the players (and more obvious what's going on!), and reworking the score card UI so players can more easily figure out who's winning (and thus whose computer to break).

I spent this week squashing the few bugs that have been popping up in QA, as well as lending a helping hand to Tony and Andy when they've needed it. I created a `DataHolder` class that helps keep track of game data between rounds, which will be the start of our outer game loop framework, and I started the `Interruptable` framework. For now, I'm going the fast and easy, if not sloppy, route to get it working and tested first before spending the time to refactor it into the current task system. I would rather not waste the time to implement it cleanly and have QA rip it out of the game.

This coming week, I'm hoping to finish up interruptables, and I'm hoping Tony and Andy can clean up transitions between scenes, and start the accolade system. I feel like morale fell slightly this week (whether because of lack of visual progress, the plague that's going around, or simply unenthused to tackle the problems at hand), so I'm hoping to give them (and the rest of the team) more freedom to decide what they want to work on.

Expect gifs and code next week :)

In other news, [David](https://gauzewave.wordpress.com) and I are creating a particle system container in Game Physics, and it's shaping up quite nicely. Expect to see a blog post on it soon.
