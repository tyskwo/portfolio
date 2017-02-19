---
title: "Hang on, I need to answer the phone."
description: "I just hate when I'm interrupted."
---

Interruptables are now in _Office Mayhem_. They are a new type of task, with a few additional rules. A single player is assigned them, and the color of the card is that player's color. If they complete the interruptable before the timer expires, they're safe. If they let the timer run out though, they lose the number of points on the card. Furthermore, if another player 'steals' it, the player who stole it _gains_ points, and the player it was assigned to loses points.


![interruptable-example]({{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/02/interruptable.gif){: .align-center }


Bringing interruptables to testing has seemed to be positive so far, but it is still in its early stages. There is a bug where the timer doesn't always start, rendering the interruptable uncomplete-able, but I hope to have that fixed for this coming week. We are happy to report that interruptables are doing their assigned job of making sure players are looking at the task cards and not just completing tasks that they think will be active. Up until this point, we only had four tasks, so the chances of completing a random task and being awarded points for it was a high probability, but as we add more tasks, players will have to look at what tasks are active. Interruptables are making players do this, and the player don't seem to think it's unfair, which is great for the roadmap of new tasks we want to implement.


<figure class="align-center">
 <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/02/interruptable-steal.gif">
  <figcaption>Interruptables can be stolen - the player who steals it gets points.</figcaption>
</figure>


In other news, [Andy](http://andrewmillsapblog.wordpress.com) has completed our long-overdue intro sequence. Before each round starts, the camera is zoomed out, showcasing the entire level (and giving players an opportunity to locate all of the items in the level) before displaying what 'day of the week' it is and zooming in. It's a small change, but it adds so much to the flow between levels, and players have really been appreciative of being able to glance at the entire level before being put into the action. Andy also has updated our `ScoreCard` UI so that it is easier to see which player is in the lead without having to read all of the scores – it's like a reverse cell service icon.


![scorecard-example]({{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/02/scorecard-example.gif){: .align-center }


[Tony](http://tonyl.info) fleshed out the framework for our accolade system. Right now, all it keeps track of is number of tasks completed and number of items sabotaged, but the framework is complete, which was a pretty large undertaking. The next task obviously is to create the actual accolades on the end screen and to award them to players. The team is still trying to figure out the best way to do this. On one hand, we really want to have in-office trophies the color of the player that won it, but we don't know an easy way to distinguish the different accolades that way. Tony is also working on a transition framework to better hide the switching of Unity scenes.
<figure class="align-right" style="width:350px">
 <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/02/charlie-run.gif">
  <figcaption>He's on a treadmill right now, but soon enough he'll be running through the office.</figcaption>
</figure>
For instance, on the start menu when all of the players crowd around the door to go to the first level, everything freezes as Unity's `SceneManager` class loads the `level_1` scene. We hope to mask this by putting some sort of fade/screen-wipe, etc. on either end of loading a scene. It's a polish mechanic, but I think it would add a lot to the professionalism of _Office Mayhem._

The art team has been hard at work this past week. [Norman](https://npaquettewordpress.wordpress.com) has finished our first character, Charles, and [Andrew](http://andrewmessier.blogspot.com) has finished animating him. I'm excited to get these into the build. [Will](https://willconcannonart.com) has been hard at work cleaning up some of the pre-existing assets (like the computer and outbox) to make sure they read the better in the office, and creating new assets like the coffee pot, bathroom door, and server rack (Wait, a server rack? Yes, stay tuned).

<figure class="align-right" style="width:200px">
 <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/02/upcomingWork.png">
  <figcaption>If you are Sherlock Holmes, you might be able to figure out some of our planned features on the left.</figcaption>
</figure>

This coming week we plan on cleaning up a lot of smaller polish-level issues we have been neglecting over the past couple of weeks. While doing that, the programming team also hopes to clean up some of the rough edges of the codebase, specifically relating to the way `Interruptables` are handled on the back end, and smaller changes to the elevator, accolade, and message systems. The artists are cranking away at new environment assets, characters and animations, and the design team is going to be hard at work creating and balancing new tasks and laying out new levels. The end of the semester is fast approaching, and we're super excited to see how much (and what) we can add to _Office Mayhem_ before then.

Also, I mentioned [a couple of weeks ago]({{ site.url }}{{ site.baseurl }}/2017/graduation-t-14-weeks/) that I was working on transferring my website from Wordpress to my own self-hosted solution. Hopefully you noticed the site is a little different? I'm super excited because I finally have full control over things like the CSS, asset management, etc. In my opinion, this new site is a lot cleaner. Have a look around!
