---
title: "Obstacle Avoidance"
excerpt: "A 'feeling' way to avoid things."

header:
  image: /assets/images/portfolio/2015/obstacle-avoidance/feature.png
  teaser: /assets/images/portfolio/2015/obstacle-avoidance/feature.png

language: "Swift"
year: "2015"

sidebar:
  - text: "## 2015, Swift ##

### Description ###

A tech demo where 'boids' avoid obstacles by using a set of 'feelers' to see their immediate surroundings.

### Outcomes ###

Swift: optionals, guards, etc.


SpriteKit.


Object pooling.


### Links ###

[Source](mailto:me@tyskwo.com){: .btn}
[Executable]({{ site.url }}{{ site.baseurl }}/assets/downloads/portfolio/2015/obstacle-avoidance/Obstacle Avoidance.app){: .btn}
"
---

![raycast-intro]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/obstacle-avoidance/intro.gif){: .align-center}


This project was an exercise in two parts: in AI-based obstacle avoidance, and in using SpriteKit best practices. The demo has three modes to explore different uses of raycast obstacle avoidance: a generic common approach, a reversed 'follow/block' approach, and a 'field condition' approach.

### The Rays ###

Each ray is a CGPath, which is then added to an SKSpriteNode. Each node has a physics body which has its contact bit designator set to detect contact with rocks, but not have a solid body attached to it. This SKSpriteNode is then added to the parent node (the shape), which has its own physics body which is what forces act upon. This allows the rays to be feelers of sorts.

{% highlight swift %}
//triangle initialization
init()
{
    super.init(type: .Triangle)

    //add the rays
    rays.append(Feeler(type: .AngleDependent))
    rays.append(Feeler(collideImpulse: Shape2.Impulse.Down))
    rays.append(Feeler(collideImpulse: Shape2.Impulse.Up))

    //set the rotation
    rays[1].zRotation =  Logic.Boid.angle / 180 * 3.14
    rays[2].zRotation = -Logic.Boid.angle / 180 * 3.14

    //attach the rays to self
    for ray in rays
    {
        ray.zPosition = self.zPosition - 1
        ray.attachTo(parent: self)
    }
}
{% endhighlight %}

The demo also has a debug mode (by pressing 'D' on a QWERTY keyboard), which shows the values for the amount of rocks (when applicable), angle of the rays, force during ray contact, simulation speed, and length of the rays. The length is updated on newly spawned shapes. The rays turn red during contact with a rock to show their logic. In the above code snippet, you see `Logic.Boid.angle`: all of the data for the boid types is saved and loaded from a plist file:

{% highlight swift %}
/** Save the current parameters as the new defaults */
func saveDefaults()
{
    //get the plist file
    let path = NSBundle.mainBundle().pathForResource("BoidParams", ofType: "plist")!
    let plist = NSMutableDictionary(contentsOfFile: path)! as NSMutableDictionary

    switch currentMode
    {
    case .triangle:
        let triangleDictionary = plist.objectForKey("Triangle")!

        triangleDictionary.setValue(Logic.Boid.angle,         forKey: "angle" )
        triangleDictionary.setValue(Logic.Boid.impulseAmount, forKey: "force" )
        triangleDictionary.setValue(Logic.Boid.rayLength,     forKey: "length")

        plist.writeToFile(path, atomically: true)

    default: func doNothing() {}
    }
}
{% endhighlight %}

![raycast-debug]({{ site.url }}{{ site.baseurl }}/assets/images/portfolio/2015/obstacle-avoidance/debug.gif){: .align-center}


### The Modes ###

<figure class="half">
    <img src="/assets/images/portfolio/2015/obstacle-avoidance/mode2.gif">
    <img src="/assets/images/portfolio/2015/obstacle-avoidance/mode3.gif">
</figure>
As stated earlier, there are three modes in the demo, to explore different applications of this. Most (video game) uses are for enemy AI avoiding obstacles (hence the obvious name), but I wanted to see if there were other possible applications throughout games. The first is a generic three-ray approach that simply the avoids the incoming rocks. The second mode, using the same logic as the other two, tries to stay behind the randomly moving rock. This could act as a following or blocking behavior, depending on the perceived direction of travel.

This could be useful in racing games when the player tries to overtake opponents, like in many arcade racing games, or for NPCs to follow the payer. The third mode serves two purposes, really. Firstly, it is an upgraded first mode, with five rays as opposed to three, which shows how much more successful two extra rays can be. The third extra ray, the circle around it, acts as a 'field' (not the physics type), which in this case causes the shape to shrink in a last-ditch effort to avoid getting hit. This could be used in other ways, like a crude distance-based AI state machine with multiple rings.

### Retrospection and Postmortem ###

Being my first full project in Swift, the code isn't the neatest, and given the time frame for the project, the concepts might not be fully-realized, but this is a good representation of what the technique is capable of. Furthermore, this project is what I submitted for a 2016 WWDC scholarship – <a href="https://twitter.com/tyskwo/status/729832151320035328">which I am proud to say I was awarded.</a> I hope to one day go back and explore raycasting some more – there are a ton more applications floating around in my head, maybe a few of them will be successful in application. Also, post-completion, I found a memory leak associated with SKShapeNode allocation – <a href="http://stackoverflow.com/questions/27883162/does-skshapenode-still-leak-memory">this seems</a> <a href="http://stackoverflow.com/questions/18889297/skshapenode-has-unbounded-memory-growth">to be</a> <a href="http://sartak.org/2014/03/skshapenode-you-are-dead-to-me.html">reported elsewhere,</a> and hopefully Apple fixes it at WWDC this year.
