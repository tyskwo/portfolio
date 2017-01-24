---
layout: post
title: "Design Insight"
comments: false
description: "A little look into how I go about designing games, with a little example at the end."
keywords: "lines, design, minimalist, experimental"
---

So in my last (and first) post, I went into detail on how I personally design games.  And what better than to, like my wood shop teacher in junior high always said, show by example.

I created a small prototype over the past two weeks for my Game Technology class. The guidelines were simple: create a horizontal scroller. Like my past projects though, I wanted to test the boundaries, and see how abstract I could make the topic. The first thing I did was ask what constitutes a horizontal scroller? Obviously some sense of horizontal movement, but what needs to move to give a good sense of this right-to-left movement. Through some intense research (read: Google Images), I found that generally the background and enemies move, while the player stays stationery with regards to the x-coordinate. I thought that was too simple though, and it was just a 90 degree rotation of my previous project, [*Blur*]({{ site.url }}portfolio/blur/). So I decided to take things one step further.

I made the background as simple as possible. A sine wave slowly moving across the screen. The enemies are simple too, little blocks with arms. The player is simple too, a little arrow, but at the same time it's not. It has free movement over the screen. It has an ever-changing tail. It constantly moves forward. It's different.

When the player overlaps its tail, it creates a barrier that stops enemies. At first, they were bombs to be detonated by the player, but it just wasn't fun, and way too enemies made it across the screen (and thus slowly killing the sine wave). The larger the shape created by the overlap, the larger the barrier, and the more enemies it can take before it disappears. The player's tail regrows first by enemies hitting barriers, and secondly by the player hitting enemies. Barriers give more bang for the buck, for overlapping your tail is the main mechanic of the game, but I figured the player needed a way to be able to single-out enemies about to hit the edge of the screen.

Speaking of which, when enemies do hit the edge of the screen, they deplete the sine wave, and in order to repair it, the player needs to pick up shrunk barriers; after a while they turn red and shrink. They still work and can block enemies, they're just smaller. The key though is that the larger the number, the larger the sine wave gets repaired. But at the same time, the player is getting rid of a barrier, so there's a fine balance to be played with.

The best way to experience first hand is to obviously [play it]({{ site.url }}portfolio/lines/). I feel it is a deceptively deep game. There are still some balancing issues and design flaws, but I am happy with the uniqueness of this prototype, and hope to return to it someday.

{: height="840px" width="418px" style="image-align: center"}
![Lines gif]({{ site.url }}assets/images/blog/2014/02/linesmidgame.gif)
