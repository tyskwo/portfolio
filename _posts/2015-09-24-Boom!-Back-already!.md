---
layout: post
title: "Boom! Back already!"
comments: false
description: "Flocking is a mesmerizing AI algorithm. If only I can find a screensaver that utilizes it..."
---

Look at that - less than two weeks! School has been going swimmingly so far. The weather is starting to cool down, the days are getting a little shorter, and the work is getting a little harder.


In Game AI over the last two weeks, we've been implementing different steering algorithms. This week, we took our jab at simulating flocking - a set of algorithms to simulate a 'hive mind' of sorts, like how birds fly in groups, or fish swim in the sea. Here is mine, not too shabby if I do say so myself:


<iframe width={{ $video-width }} height={{ $video-height }} src="https://www.youtube.com/embed/SFF9pqgZRtY?rel=0" frameborder="0" allowfullscreen></iframe>


The demo features the ability to add/delete unit, as well as mess around with the weight and radius of the three sub-steerings. A stands for alignment, C for cohesion, and S for separation. The behavior behind the steering algorithm is really cool:

{% highlight cpp %}
Steering* FlockingSteering::getSteering()
{
    Vector2D align =
            mpAlignment->getSteering()->getLinear() * gpGame->getFileManager()->getAlignmentWeight();
    Vector2D cohesion =
            mpCohesion->getSteering()->getLinear() * gpGame->getFileManager()->getCohesionWeight();
    Vector2D separation =
            mpSeparation->getSteering()->getLinear() * gpGame->getFileManager()->getSeparationWeight();

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


This function calls each of the three sub-steering algorithms: alignment to the group, cohesion to the group, and separation from the group, and then combines the returned linear velocity vectors. Using this combined vector, we divide it by the total weight and normalize it, and then multiple it by the maxAcceleration to keep the velocity constant. Super simple!


The sub-steerings are also super simple. For instance, here's the Alignment steering:


{% highlight cpp %}
Steering* GroupAlignmentSteering::getSteering()
{
    mpNeighbors =
       gpGame->getUnitManager()->getNeighbors(mpMover->getPosition(), gpGame->getFileManager()->getAlignmentRadius());

    Vector2D linearVector = (0, 0);

    if (mpNeighbors.size() > 0)
    {
	    for(unsigned int i = 0; i < mpNeighbors.size(); i++) { linearVector += mpNeighbors[i]->getVelocity(); }

	    linearVector /= mpNeighbors.size();
	    linearVector.normalize();

	    mLinear = linearVector;
    }

    return this;
}
{% endhighlight %}




For each of the sub-steerings, we find all the other boids within the given radius, mpNeighbors. As long as there are some neighbors, we find their average velocity, normalize the resulting vector, and set it equal to our velocity. Like I said, super simple stuff.


That being said, it did take me longer than I'd like to admit to get it to this point, but it's functional, decently good-looking, and sturdy.

[Feel free to try it for yourself!](https://www.dropbox.com/s/k4z45sseapz3lgt/flocking.zip?dl=0) Mess around with the different values, and comment if you find anything interesting!
