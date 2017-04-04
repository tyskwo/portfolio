---
title: "The week of too many hours."
description: "The work train has no brakes."
---

It was a very busy week in the cubicles of _Office Mayhem_, but with work comes progress. With only three weeks left, we could use as much progress as we can get our hands on.

The biggest addition is the introduction of timer tasks: a group-wide task where all of the players fight over doing the most of whatever the task dictates (like throw everything out, or answer the most phone calls). When the timer runs out, points are awarded to first, second, and third place. They have been received really well at QA, and experienced players love the new strategy elements they bring to the game.

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/04/timerUI.gif">
  <figcaption>Only two players, but here's a timer task in action. With four players, it is a hectic dash to come in first.</figcaption>
</figure>

[![iteration]({{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/04/timerIteration.png){: style="width: 350px" .align-right}]({{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/04/timerIteration.png)
The UI for timer tasks fell on my shoulders, and it was a nice change of pace for me. With that said, it came with its fair share of difficulties: we wanted to introduce all of this new functionality to our pre-existing `TaskCard` framework, while still letting the players get the information they needed at a glance. The finished product is above, obviously, but there was a fair amount of iteration to get there.

Another feature added this week was a plug-in 'attract mode' – basically a kiosk mode if the game sits idle for too long. We also knocked out a lot of the bugs and polish items we've been meaning to get to. [Andy](http://andrewmillsap.weebly.com) added random character creation on the start screen which is a nice bit of polish, and [Tony](http://tonyl.info) finished the stamp system in the break room, which finally finishes our inner game loop.

This coming week, we hope to spruce up our tutorial systems so new players experience even less friction when playing the game, and to implement the accolade system and Employee of the Week room to finally be feature complete. After that it would be small ticket items like implementing new sounds, tasks, and characters. The finish line is really close, and I can't wait to cross it.
