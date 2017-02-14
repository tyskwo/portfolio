---
title: "The Many Uses of SKConstraints"
description: "At WWDC 2014, SpriteKit introduced `SKConstraints`: a really nifty feature that is akin to `LookAt` constraints in SceneKit."
---

At WWDC 2014, SpriteKit introduced SKConstraints: a really nifty feature that is akin to LookAt constraints in SceneKit. There are three main types of constraints: rotation, distance, and position. Constraints consist of two pieces: its type and an SKRange designating how much leeway the constraint has.

Here's a quick example that shows how to create a node (followerSprite) follow another node (leaderSprite):

{% highlight swift %}
//create a range to designate how far of a
//distance to follow from.
let distanceRange = SKRange(lowerLimit: 100.0,
                            upperLimit: 150.0)

//create a range to orient itself to the other node
let orientationRange = SKRange(constantValue: CGFloat(M_2_PI*7))

//create the constraints
let distanceConstraint
= SKConstraint.distance(distanceRange, toNode: leaderSprite)

let orientConstraint   
= SKConstraint.orientToNode(leaderSprite, offset: orientationRange)

//add them to the follow sprite
followerSprite.constraints = [orientConstraint,
                              distanceConstraint]

    //the constraints will be applied in the order
    //they are placed inside the array. in this case,
    //the follower will orient to the leader before the check
    //for distance. this can cause some adverse effects,
    //so be wary of the order.
{% endhighlight %}

Note that the constraints are applied after the physics simulation is run, which is great because the constraints will then always be followed. Here are links to the [SKConstraint class reference](https://developer.apple.com/library/ios/documentation/SpriteKit/Reference/SKConstraint_Ref/index.htm) and the [SKRange class reference](https://developer.apple.com/library/ios/documentation/SpriteKit/Reference/SKRange_Ref/index.html); there are multiple inits for each, so there is a lot of flexibility within. [Ray Wenderlich has a great tutorial](https://www.raywenderlich.com/80917/sprite-kit-inverse-kinematics-swift) on inverse kinematics with constraints through the scene editor in Xcode – check it out if you'd like to learn more.
