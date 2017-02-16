---
title: "Flocking"
excerpt: "Sheep in a field, fish in the ocean. People in a crowd?"

header:
  image: /assets/images/portfolio/2015/flocking/feature.png
  teaser: /assets/images/portfolio/2015/flocking/feature.png

language: "C++"
year: "2015"

sidebar:
  - text: "## 2015, C++ ##"


  - title: "Description"

    text: "Sheep in a field, fish in the ocean. People in a crowd? The classic AI steering algorithm."


  - title: "Outcomes"

    text: "Flocking.


    Allegro5.


    Using a third-party framework."


  - title: "Links"

  - button: Executable
    link: /assets/downloads/portfolio/2015/flocking/flocking.zip
    icon: download
---


This is my implementation of Craig Reynold's famous [flocking algorithm.](https://en.wikipedia.org/wiki/Flocking_(behavior))

### The Setup

The demo has a base boid class, which has a linear and rotational velocity, an acceleration, and maxes for all associated values. Each boid also has a steering object, which can be any available steering in the program, and in this case, is the flocking steering, which has three sub-steerings: alignment, cohesion, and separation.

<figure class="align-center">
  {% include video id="151457337" provider="vimeo" %}
  <figcaption>Note: Running Quicktime on OS X, recording Windows 7 through VMWare is not optimal. The project runs cleanly at 60fps when my Macbook isn't dying with Quicktime recording.</figcaption>
</figure>


### The Algorithm
The behavior behind the steering algorithm is really cool:

{% highlight cpp %}
Steering* FlockingSteering::getSteering()
{
	Vector2D align =
            mpAlignment->getSteering()->getLinear()
          * gpGame->getFileManager()->getAlignmentWeight();
	Vector2D cohesion =
            mpCohesion->getSteering()->getLinear()
          * gpGame->getFileManager()->getCohesionWeight();
	Vector2D separation =
            mpSeparation->getSteering()->getLinear()
          * gpGame->getFileManager()->getSeparationWeight();
	Vector2D combined;

	combined += align + combined + separation;

	combined /=
           gpGame->getFileManager()->getAlignmentWeight()
         + gpGame->getFileManager()->getCohesionWeight()
         + gpGame->getFileManager()->getSeparationWeight();

	combined.normalize();
	combined *= mpMover->getMaxAcceleration();
	mLinear = combined;

	return this;
}
{% endhighlight %}

This function calls each of the three sub-steering algorithms: alignment to the group, cohesion to the group, and separation from the group, and then combines the returned linear velocity vectors. Using this combined vector, we divide it by the total weight and normalize it, and then multiple it by the maxAcceleration to keep the velocity constant.

The sub-steerings are also super simple. For instance, here's the Alignment steering:

{% highlight cpp %}
Steering* GroupAlignmentSteering::getSteering()
{
    mpNeighbors =
       gpGame->getUnitManager()->getNeighbors(
           mpMover->getPosition(),
           gpGame->getFileManager()->getAlignmentRadius());

    Vector2D linearVector = (0, 0);

    if (mpNeighbors.size() > 0)
    {
	    for(unsigned int i = 0; i < mpNeighbors.size(); i++)
            { linearVector += mpNeighbors[i]->getVelocity(); }

	    linearVector /= mpNeighbors.size();
	    linearVector.normalize();

	    mLinear = linearVector;
    }
    return this;
}
{% endhighlight %}

For each of the sub-steerings, we find all the other boids within the given radius, mpNeighbors. As long as there are some neighbors, we find their average velocity, normalize the resulting vector, and set it equal to our velocity. Like I said, super simple stuff.

### Retrospection and Postmortem
This project was at the tail end of my steering algorithms section. By this point, I have a good handle on steering algorithms, from avoidance, follow, and wander, to more advanced like this and other weighted algorithms. In fact, DASHockey uses multiple steering algorithms in a weighted fashion for the opponent AI. This also was a good exercise in optimization – I can get up to 300 boids on my machine before the frame rate starts to suffer, and I believe it is an issue with how I call Allegro5's draw functions, not the AI behavior.
