---
title: "Color Hacker"
excerpt: "A tile-based strategy game with lots of animation."

header:
  image: /assets/images/portfolio/2015/color-hacker/feature.png
  teaser: /assets/images/portfolio/2015/color-hacker/feature.png

language: "AS3"
year: "2015"

sidebar:
  - text: "## 2015, AS3 ##


### Description ###


A tile-based strategy game with a focus on easing.


### Outcomes ###


Learned Starling.


Tweening and easetypes.


Function callbacks.


### Links ###


[Executable](https://www.dropbox.com/s/60zjp6qn62b57ct/Color%20Hacker.zip?dl=0){: .btn}
"
---

![color-hacker-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/color-hacker/intro.gif){: .align-center}

Color Hacker was my first true team-based experience with game development. Working with a designer, artist, and producer, we had three weeks to prototype and present a turn-based strategy game. For our group, the result of these three weeks was _Color Hacker_ – a game in the vein of Galcon and Chess. Being the programmer, I was responsible for the code behind the prototype, as well as technical documents and communicating with the team on what was feasible during the three week time period.

_Color Hacker_ is a territorial control game in which each player takes on the role of a hacker trying to break into a supercomputer. Each player controls units that can move around the circuit board. As they move around, they leave a colored trail behind them. Each time the player moves over a tile they own, the tile gets upgraded a level. This makes the tiles more valuable and more difficult for the enemy to capture. Players can destroy their opponents territory by going over it, in a similar manner to which they upgrade their tiles. Each player also selects weapons at the start of the game, which they can use either after two turns or four turns. These weapons dramatically change the flow of gameplay. After 12 turns, the game ends, with points being awarded based on the tiles the player controls, who had the most tiles, and who had the highest leveled tile. The player with the most points is the winner.

### Teamwork

We had two, two hour meetings each week for the duration of the project. Each team member was required to log 24 hours of work, and our team logged 146 in total. We worked together to create a game that was coherent and complete and went above and beyond the requirements. During out meetings, I lead discussions on the technical aspects of the prototype and communicated with the artist and designer to determine what was feasible to include in the prototype within the timeframe of the project. We used daily scrum cycles to stay on schedule, and broke the project into three sprints.

![color-hacker-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/color-hacker/rules.gif){: .align-center}

### The Prototype
Color Hacker is written in AS3 using the Starling framework, with heavy use of its easing system. Each menu system runs off of a base 'screen' skeleton:

{% highlight as3 %}
bool isEntryFinished = false;

Screen()
{
    //..... initialize everything
    isEntryFinished = animateIn();
}

public function animateIn():bool
{
    //..... all the entry animations
    lastOne.onComplete = function():void { return true; };
}

public function update():void
{
    if(isEntryFinished) { }
}

public function checkInput(e:KeyboardEvent):void
{
    if(e.keyCode == 'Q') animateOut();
}

public function animateOut():void
{
    //..... all the exit animations
}
{% endhighlight %}

This allows the code to be very modular, and the screens to share base animation code through inheritance. Animations can be abstracted to be reused, like specific combinations of easing function and timing, rather than having to recreate them. This allows for the game to have an overall feel that can easily be changed; for instance from 'floaty' to 'bouncy' to 'harsh.'

### Retrospection and Postmortem

Most people tend to hate on group work, and not just in this industry. From coworkers to study buddies and everything in between, groups always seem to collapse, but the group behind Color Hacker was great. We worked well together, and had many similarities in our senses of game design, and art style – we shared a common vision for the game which made it very easy to execute. This project also allowed me to brush up on my scrum methodologies, as well as document writing.

![color-hacker-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/color-hacker/weapons.gif){: .align-center}
