---
title: "Boost"
excerpt: "My first game; Pong with a twist."
header:
  image: /assets/images/portfolio/2013/boost/feature.png
  teaser: /assets/images/portfolio/2013/boost/feature.png

language: "Java"
year: "2013"

sidebar:
  - text: "## 2013, Java ##"


  - title: "Description"

    text: "My first game; Pong with a twist. Built over 48 hours for a game jam."


  - title: "Outcomes"

    text: "Finishing a game."


  - title: "Links"

  - button: Executable
    link: /assets/downloads/2013/boost/Boost.jar
    icon: download
---

Boost was my first completed game, finished back in November of 2013, with no engines used – it's built completely from scratch. It was for a 48 hour game jam in my college hall, with the theme of ‘pong, but better.’ Not only my first completed game, Boost was also my first time participating in a game jam. Boost features pixel perfect collision detection, basic enemy AI, and a simple wave system for enemy generation. Technically it is a two player game, but can be played by one player too.

### Collision Detection

Sadly, the source code to Boost was lost with a broken hard drive, but I was proud of the modularity of the collision code I had implemented. The method passed in two rectangles, and did a preliminary check of collision, and if the rectangles were colliding, it checked the alpha level of the collided pixels. Something likes this:

{% highlight java %}
public bool checkCollision(Rect r1, Rect r2)
{
    if(r1.topLeftX  < r2.topRightX && r1.topRightX > r2.topLeftX  &&
       r1.topLeftY  < r2.topRightY && r1.topRightY > r2.topLeftY)
    {
        //collision detected, so return true
        return true;
    }
    else
    {
        return false;
    }
}

. . .

public void checkBallCollision(Paddle paddle, Ball ball)
{
    if(checkCollision(paddle.boundingBox, ball.boundingBox))
    {
        if(checkPixelCollision(paddle.texture, balls.texture))
        {
            //code to determine ball collision vector
        }
    }
}
{% endhighlight %}

What’s nice about these methods is that they’re very lightweight, allowing me to also perform collision detection between the balls themselves. After determining collision between the ball and the paddle, the collision detection then took into account the place of collision, which determined the speed and direction that the ball would bounce off at. Some pseudocode:

{% highlight java %}
let distance  = (paddle.middleY - ball.middleY) / paddle.halfLength;
let angle     = distance * PI/2;

let velocity.x =  cos(angle);
let velocity.y = -sin(angle);
{% endhighlight %}

### Enemy AI and Wave System

Each ball 'enemy' has one of three AI templates attached to it – normal, weave, or bullet. Normal is your normal pong ball, it travels in a straight line at a constant speed. Weave balls travel in S-like patterns moving up and down periodically. Bullet balls travel in a straight line, but spawn in a location where the center wall isn't destroyed, and they gain speed the closer they get to the wall. The wave system for the enemies works on a timer that counts both up and down, like a sine wave, and more enemies spawn the closer the timer is to 0 and its max value, creating these periods of hectic movement and calmness.

<figure class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2013/boost/example.gif" alt="">
  <figcaption>Notice the ball collision in the center in the beginning – off the paddle and another ball.</figcaption>
</figure>

### Other Tidbits

Boost is definitely rough around the edges, but it is complete. The boost system increases the players' movement speed, which is both a blessing and a curse. The collision detection between the balls is satisfying to see in action, as is seeing balls bounce off the back of the paddles. The power-ups are a good addition, especially the slow down one, but in terms of balance, they are definitely underpowered.

### Retrospection and Postmortem

Looking back two years postmortem, I am proud of what I was able to accomplish with Boost. Most of my prototypes before this were tile-based games, with no enemy structure, and very little in terms of complexity. Between the structure of the enemies, their AI, the collision detection, and the wave system, I would consider this the peak of my knowledge at the time. Although the source code is lost, I do remember being proud of how I structured the game's systems – it wasn't a big pile of spaghetti when it was all said and done. One of Boost's dark spots is that the collision detection sometimes goes haywire and random enemies disappear. Oh well.
