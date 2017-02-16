---
title: "Planker Pro"
excerpt: "An online, asymmetrical game where you plank, or nab the plankers!"

header:
  image: /assets/images/portfolio/2016/planker-pro/feature.png
  teaser: /assets/images/portfolio/2016/planker-pro/feature.png

language: "Unreal"
year: "2016"
portfolio: "true"

sidebar:
  - text: "## 2016, Unreal ##"


  - title: "Description"

    text: "An online, asymmetrical game where you plank, or nab the plankers!
"


  - title: "Outcomes"

    text: "Unreal.


    Networking in Unreal.


    Project refactoring.


    Designer-oriented tools."

---

_Planker Pro_ is the final project from my Production II class. In a world where planking is illegal, defy the cops chasing you and plank as much as you can! Plank on top of dumpsters and park lights, traffic cones and hot dog carts. As a part of a team of seven, I was brought on the team to supply the networked back-end as well as create designer-oriented systems to aid in lowering level creation times and increase the rate of prototyping new mechanics. This project was produced in a scrum environment.

{% include video id="8FGeg6tkH88" provider="youtube" %}


### Systems ###

For the class, the project was 10 weeks in length, with the game reaching beta by week 10. I joined the team at the end of week 3, right before alpha. At the time, the game was more or less a tech demo: there was no cop implemented, the city wasn't finished, and there wasn't any type of multiplayer; the planner was able to plank on different objects. Upon joining the project, I quickly went to work on tidying up the existing code base so that it would be easier to implement networking, as well as make it easier for the designers to more quickly flesh out the level. Working in blueprints (not my decision, and we didn't have a long enough timeframe to covert to C++ against my best attempts), I created a system for the designers to be able to tag objects as 'plankable' and give them a category and position, with easy copying of objects. Before this, they would have to individually tag objects, and copying wasn't an option due to how the plank position was saved. This very quickly increased the speed at which the level was created, allowing the designers to focus on other mechanics, such as the cop's Segway.

![planker-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2016/planker-pro/map_overview.png){: .align-center}


### Networking ###

The backbone of the networking system is a Server-Client model, with the cop also hosting the server. Unreal's default setup (unless I am sorely mistaken), is that one client also runs as the server (a dedicated server option is also available). Because of the desire to be able to host rooms, and nodding towards the asymmetrical gameplay, I made it so that the cop would automatically host the game. This could be considered a design flaw – that the cop doesn't change players during a room session – but that was a limitation I had to make given the aggressive timeframe. On the other hand, one could argue that you get to choose whether or not one wants to be a cop or planker beforehand, which is good in some regards.

Replication in Unreal is awesome. They have such an easy workflow, though at times it was difficult to get working right, but that was only due to my own oversights. Timers, animation states, and character movement are super easy to sync up across clients. The hardest part was trying to balance what needed to be prioritized and how to create a balance between network lag, interpolation, and smooth, realistic movement. The 'normal' online problems.

![planker-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2016/planker-pro/planking.png){: .align-center}


### Retrospection and Postmortem ###

I learned two things during this project: networking is still difficult, even in Unreal, and blueprints are not an equivalent to writing code. There were too many times to count where I was wrangling with the limitations of blueprints. At times they were useful for interfacing between different objects in the game world and such, but they are just slow and limited in their flexibility. They're nice for prototyping, and overall very intuitive. I also had my first experience in creating tools for designers (outside of myself). This was great experience as it allowed me to be in constant communication with the design team, as well as understand their workflow and create tools that aid in that workflow.

I was brought on the project to add online multiplayer in two weeks with an existing code base, and then provide bug fixes and support for the remainder of the project. I succeeded in both of those goals. I still found it challenging, especially in blueprints (again, SO so limited), but I am proud of what I was able to accomplish.
