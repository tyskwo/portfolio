---
title: "SHMUP"
excerpt: "Architecture at its finest."

header:
  image: /assets/images/portfolio/2015/architecture/feature.png
  teaser: /assets/images/portfolio/2015/architecture/feature.png

language: "C++"
year: "2015"

sidebar:
  - text: "## 2015, C++ ##"


  - title: "Description"

    text: "A simple vertical shooter with a heavy emphasis on strong architecture."


  - title: "Outcomes"

    text: "Design patterns.


    Flyweights.


    Object pooling.


    File input/output.


    SFML."


  - title: "Links"

  - button: Executable
    link: /assets/downloads/portfolio/2015/shmup/SHMUP.zip
    icon: download
---

SHMUP is a vertical shooter, featuring multiple difficulties, enemy AI, loading from text files, and a complete, structured, underlying architecture based on an event system. The game is wrapped around SFML function calls, which allows the underlying engine to be swapped out very easily, with Allegro5, SDL, and even DirectX possible with a little more effort. This project features many [programming patterns,](http://gameprogrammingpatterns.com/contents.htm) including flyweights, managers, an event system, menu states, a variable time step game loop, object pools, and double buffering.

![shmup-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/architecture/example.gif){: .align-center}

### The Patterns in Action

![class-list]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/architecture/class_list.png){: style="width: 300px" .align-right}

At the project's center is an event system, which consists of events and event listener. Each manager throughout the codebase can create event listeners, which as their name invokes, can listen for when a certain event is fired. Likewise, these managers can also fire events to signify something has happened. All of these events and event listeners are parsed through the central hub of the event system, that way the listeners can know when their corresponding event has been fired. The managers, in turn, perform an action when the event listener registers an event. This action depends on the listener, such as spawning a new enemy, or pausing the game, or firing a bullet, or playing a sound.

The menu system is a state machine with each menu being a state. When the game is started, the menu system is ignored, and then is changed to the 'game over' state when the game ends. Both the bullets and the enemies are an example of object pools, with the bullets sharing resources so not to bog down the rest of the game if the player decides to spam the fire button. Similarly, the background uses a flyweight pattern to hold multiple of the texture to simulate scrolling while keeping only one reference to it in memory, reducing the load for said feature.

The input manager wraps SFML's key bindings to fire events through the custom event system, which are then picked up by other managers throughout the game.

The load/save manager write and load the game state from a text file, using `std::cin` and `cout`.

The enemy manager controls the enemy spawning and AI, while the collision manager works out the collision between the bullets and ships.
 
### Retrospection and Postmortem

This project, a) speaks for itself – take a look at the source code to gain a better understanding of the architecture, and b) single-handedly made me realize the structure of systems and layouts of larger games. It has given me insight on how to better set up the architecture of games, and has given me the confidence to tackle larger projects without fearing the cluttered mess of a mis-managed codebase. Looking back almost a year later, there are many things I'd do differently - including, but not limited to: how we handled the HUD, a more robust state machine for the menus, a combined save/load manager, and better resource management for the audio through a flyweight or an object pool, or additive audio playing. On the other hand, the collision detection is pretty stellar. It works well, and is a decent implementation keeping it separate from the objects themselves.
