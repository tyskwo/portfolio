---
title: "A Little Bit of Relaxation"
description: "Taking a small breather from the programming-side of Capstone. Time to look at _Office Mayhem_ as a marketable game."
---

13 days left. I've taken a breather over the last two days. This past week of QA has been controls- and player feedback-centric, and we've been focusing on the core gameplay experience as opposed to adding more features to the build. _Office Mayhem_ has reached its saturation point: without a tutorial or progression, any more features added to the build would overwhelm new players and negatively affect their experience. When the faculty try _Office Mayhem_, many of them will be playing for the first time. As such, if we include new features into the build, our gameplay might feel cluttered and hard to understand. Because of this, we have a short list to implement for the rest of the semester: finish music, remake the item icons and task card UI, add more environment art to make the level seem more grounded, and add a tutorial. The first four of that list are polish, game-feel related tasks whereas the last, and arguably the largest, is to help the faculty understand how to play the game before we throw them into the mayhem. [Jeremy](http://jeremyroot.squarespace.com) has spent the last couple of days reworking the icons and building the tutorial level, and now him and I need to decide on the format for introducing the mechanics.

The past couple of days on my end has been more marketing- and backend-focused. I took some time to clean up Pineapple, organize our drive, and start the technical wiki on [GitHub](https://github.com/tyskwo/OfficeMayhem/wiki) to better document the codebase. I also spent some time working on the logo for our team; we're _9:5 Games_ and wanted a logo for our final presentation. With my love for graphic design, I decided to take on the challenge, and the team is happy with the result.

<figure class="align-center">
  <a href="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/11/9-5games.png"><img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/11/9-5games.png"></a>
  <figcaption>For those confused by the logo, the three bars are in the ratio of 9:2:5, 'nine to five.' Click to enlarge.</figcaption>
</figure>

Along with the logo and 'November cleaning,' I've been hard at work capturing game footage with Jeremy, and I've been converting this footage to gifs for our blog posts, Twitter, etc. Speaking of which, feel free to [follow me](https://twitter.com/tyskwo), I've been tweeting more lately. Through these gifs, we are trying to create snippets of gameplay that show our vision of over-the-top fun that testers have when they play _Office Mayhem._ So far I feel that we've been successful, but there are still some scenarios we want to convey, we'll probably record more footage later this week.

I've done more than just marketing mumbo jumbo though, I've rewritten the item detection system, as well as refactored much of the character movement system, addressing the main feedback we've been getting about tightening up the controls. Next up is implementing the UI changes Jeremy is making, and adding the tutorial. More importantly, that tutorial needs to be well-tested and statistically helpful in order for its impact to be felt when the faculty try out our game. Fingers crossed, the finish line is within sight.

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/11/catmouse.gif">
  <figcaption>The office chairs act as good buffers when playing cat and mouse.</figcaption>
</figure>
