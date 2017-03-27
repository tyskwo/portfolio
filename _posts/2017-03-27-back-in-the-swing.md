---
title: "Back in the swing of things."
description: "Six more weeks of mayhem. I wouldn't want anything else."
---

There's something about the haphazardness of small island urban planning that is oddly refreshing to me. Maybe it's the narrow streets, buildings right up against the side of the road, or the simpler, less rigidness of it all. Regardless, spring break has ended and it's back to the office. We have made quite a bit of progress since coming back to cold, wet Burlington.

While [Richard](http://richardkingcapstone.blogspot.com) and [Will](http://www.willconcannonart.com) were at PAX, [Tony](https://tonyl.info) was hard at work optimizing _Office Mayhem_. He managed to double our frame rate, and almost triple it on low-end Macs like mine. On higher end machines it isn't too noticeable, but it is great to be able to lower our low-end target even more, especially if we decide to release. I took spring break as just that, a break, but towards the end I spent time on a lot of the smaller polish-level items we have been meaning to get done: getting random markers from the copier, level randomization, a better item spawning system in the tutorial, etc.

<figure class="align-center">
 <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/03/whiteboard.gif">
  <figcaption>The whiteboard is still being iterated on, but it's in a very good place. Players can draw and erase, and get new markers from the copier.</figcaption>
</figure>

Once we were all back in the Green Mountain state, production was right back in full swing. [Norman](https://npaquettewordpress.wordpress.com) started working on the fifth character head, and [Andrew](http://andrewmessier.blogspot.com) was working on new animations for our 'win' room at the end of the work week, while [Tim](http://www.tim-healey.com) has been working on a third set of levels, which I can't wait to implement.  Richard has been working on lobby music for the start and break rooms, which are eerily quiet right now. The biggest change we brought to QA testing was character customization: [Andy](http://andrewmillsap.weebly.com) worked diligently to add in swappable bodies and heads, which has been a really great addition judging by the feedback from QA.

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2017/03/character_selection.gif">
  <figcaption>If you look carefully, there <em>is</em> a headless option in there. We're unsure if it will stay though – just a joke for now.</figcaption>
</figure>

I spent this week helping Andy with the rough edges of character customization, as well as implementing new particles I received from [Jeremy](http://jeremynroot.com). We finally made the elevator a breakable object, which leads to some interesting strategies. I also implemented, and removed, group tasks. These tasks worked similar to normal tasks, but they would replace all current tasks, and there would only be one of them – creating a four player rumble of sorts to complete this task. After bringing them to QA, we soon realized that they acted as a pinch point in the game flow, and could very easily let first place keep their lead in certain instances, basically creating a standstill. While fun to work around, we ultimately decided that this wasn't the type of pace we wanted, and decided to scrap it. We took the time to look deeper at timer tasks – a specific type of group tasks that want players to do the _most_ of something in a given amount of time. These don't seem to have the same pinch points as normal group tasks, so we plan on implementing these this coming week, and hoping they fare better at QA.

We also took the time this week to record some gameplay for an alpha video. Take a watch!

{% include video id="839Ih0TVC44" provider="youtube" %}
