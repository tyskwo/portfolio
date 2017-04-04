---
title: "Flip: A Gravity Game"
excerpt: "My first released game."

header:
  image: /assets/images/portfolio/2014/flip/feature.png
  teaser: /assets/images/portfolio/2014/flip/feature.png

language: "AS3"
year: "2014"

sidebar:
  - text: "## 2014, AS3 ##"


  - title: "Description"

    text: "My first released game; a puzzler in the veins of Threes and 2048."


  - title: "Outcomes"

    text: "Starling framework.


    iTunes Connect.


    Marketing."


  - title: "Links"

  - button: App Store
    link: https://itunes.apple.com/us/app/flip-a-gravity-game/id908133039?mt=8
    icon: app_store

---


_Flip_ was my first commercially released game, created with Starling in AS3. It is a puzzle game similar to [_Threes_](http://play.threesgame.com) and [_2048_](https://gabrielecirulli.github.io/2048/) where the goal is to make the board as empty as possible while placing down new pieces. In _Flip_, two hollow pieces create a half-piece, and two half pieces create a whole piece. This repeats for two sizes, small and big, and two big whole pieces destroy each other, freeing up space. The defining game mechanic is that the player can flip the board up and down, effectively turning gravity off and on, and thus changing the layout of the board, and where the player places new pieces.

### Gravity and Spawning

The tiles are held in a column-oriented 2D array that is column based. To turn gravity on/off, I simply shift each column up until there are no more empty tiles above it. I then apply eases on the sprites of the tiles to reflect their new position.

For spawning, I add one of each of the small tiles, and then take the top and bottom of each column to account for both gravity directions. If a column has none or one tile, I add none or two of the tile that it has (because it logically is both the top and bottom of that column). Then, depending on the player's score, I apply a weighted algorithm to choose between a tile that can make a match with the top/bottom of each column or a random small tile, effectively making it more difficult as the player progresses.


### Easing

_Flip_ was the project that introduced me to interpolation and easing. Starling has a built-in interpolation engine, which I used to create the effects:

{% highlight as3 %}
TweenLite.to(map[i][j], 0.3, { y: map[i][j].y - delta, ease: Cubic.easeInOut, onComplete: finishMoving, onCompleteParams: [i,j] });
{% endhighlight %}

I didn't extract/refactor the interpolation code, and thus it is sprinkled all throughout the codebase. Not best practice, but it allowed me to experiment with different 'feelings' attributed to different combinations of ease effects.

### The Final 10%

Being my first released game, _Flip_ allowed me to experience and learn all of the work that goes into a project after the final semicolon is written. I learned basic marketing techniques like SEO and basic psychology, the backend of Apple's developer portal, and pitching in person. Although not related to programming, I felt this experience was invaluable to my growth as a person and learning to be confident in myself and the product. With a grand budget of $0, _Flip_ sold a little over five hundred copies.

### Retrospection and Postmortem

It's been a couple of years since I created _Flip,_ to put this section of the page into perspective. Sales have stagnated (obviously), but I still chalk this project up to being the most beneficial for me in seeing the entire production cycle and just how difficult and rewarding it all is. It has allowed me to learn the skills to become a more rounded developer, and use these skills and tools to create better games, and pay attention to certain details throughout an entire development cycle as opposed to just the end. In fact, much of what I've learned here I applied when working on [_Office Mayhem_]({{ site.url }}{{ site.baseurl }}/work/84_office-mayhem/).
