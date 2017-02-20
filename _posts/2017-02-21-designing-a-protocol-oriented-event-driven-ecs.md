---
title: "Designing a Protocol-Oriented, Event-Driven Entity-Component System*"
description: "Super long (but descriptive) title, with way too many dashes and buzz words. *In Swift!"
---

I have decided to pick up _[DASHockey again.]({{ site.url}}{{ site.baseurl }}/2015/DASHockey-At-GMGF/)_ When I last touched it (almost two years ago), the codebase was complete garbage. I tackled the game as a foray into developing for the then-new AppleTV and to be able to present it at the _Green Mountains Game Festival_. As such, getting a complete game with AI out in a week was a pretty large task. Since I'm tackling it anew, I figured I should be a good programmer and rewrite the game from the ground up. I eventually want _DASHockey_ to have iPhone-connected remotes for true party game multiplayer, but for now let's focus on the base architecture.

This post assumes the reader has a decent understanding of Swift. Thankfully, it's a very verbose language, so it shouldn't be too hard to get the high-level architecture from it. See [Apple's Swift handbook](https://swift.org/documentation/) for a great resource on Swift.
{: .notice--info}

### The Entity

In this protocol-oriented, event-driven example, the base entity isn't too different from the [classic pattern.](http://gameprogrammingpatterns.com/component.html):


{% highlight swift %}
class Entity
{
    var components:[Component]

    // name and tag attached to this entity
    private var name:String
    private var tag: String?



    init()
    {
        components = []
        name = "NONE"
    }

    init(name:String)
    {
        components = []

        self.name = name
    }

    init(name:String, tag:String)
    {
        components = []

        self.name = name
        self.tag  = tag
    }


    func set(tag:String)  { self.tag  = tag  }
    func set(name:String) { self.name = name }


    func getTag()  -> String { return self.tag ?? "NONE" }
    func getName() -> String { return self.name          }
}
{% endhighlight %}

Here, we have a simple array of `Component` objects (which we'll get to), a name and tag to reference this entity by, and methods for getting/setting these values. But what about finding components in our bucket?

{% highlight swift %}
extension Entity
{
    // get first component of type T
    func find<T>(component: T.Type) -> T?
    {
        for item in components
        {
            if item is T
            {
                return (item as! T)
            }
        }

        return nil
    }

    // get all components of type T
    func findAllOf<T>(component: T.Type) -> [T]
    {
        var returnArray:[T] = []

        for item in components
        {
            if item is T
            {
                returnArray.append(item as! T)
            }
        }

        return returnArray
    }
}

// sample use:
let rink = self.find(component: Rink.self)!
{% endhighlight %}

Here, we use generics to be able to return any type of component (This is very similar to how Unity handles its ECS). In the singular search, we use the power of optionals to elegantly return nil if there's no such component attached to this entity. For the multi-search, we return an empty array. In the sample use, notice that we use `ComponentType.self`. The `.self` is necessary to reference the class type, and not just the member type. It's a small syntactical hiccup, but I think it is worth the power and flexibility it provides. If you're savvy, you'll notice this is in an _extension_ of `Entity`. I did this simply for organization purposes: we don't declare any new member variables, so we can place these function in an extension. I name my extensions in comments for easy finding; this one is the 'search extension'.

For adding and updating components, we use a similar extension:

{% highlight swift %}
extension Entity
{
    func add(component:Component)
    {
        components.append(component)
    }

    // update all components that conform to UpdateableComponent
    func update(dt: Double)
    {
        for item in components where item is Updateable
        {
            (item as! Updateable).update(dt)
        }
    }
}
{% endhighlight %}

Adding is simple enough, but what exactly is happening with `update`? Swift 2 added the [`where` clause](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/ControlFlow.html) – basically a filter on an array. Thus, in `update`, we are iterating over all of our components, and if the current one is an `Updateable` component, we call its `update` method. It'll make more sense in a little bit.

That, at its core, is the entity in this system. It's not very complicated, but can be easily expanded, which is always a great goal to have.

### The Component

At its root, the `Component` class has similar initializers, name and tag functions as the `Entity` class. The interesting functionality comes from the use of protocols:

{% highlight swift %}
// in Component.swift:

protocol Updateable
{
    func update(_ dt: Double)
}

protocol Renderable
{
    func addToScene()
    func get() -> SKNode
    func destroy()
}
{% endhighlight %}

This is designating that any component that conforms to the `Updateable` component will have to contain a definition for the function `update` that takes a double as a parameter (and similarly for the `Renderable` protocol). What this does, at least in my setup, is that it takes items like the players, puck, and nets, and turns them from entities that would have to be handled by the scene to components that are held by a single entity. Basically, it' just a reduction of one more layer, and it can be argued that it's just renaming the architecture, shifting it up a level, but more importantly, it allows the puck object to contain all of its own logic, broken up into extensions and protocol implementations. Yes, in games with hundreds of thousands of objects this would not be ideal, but in _DASHockey_ where there are never more than ten objects in a scene, it works very well. For instance:

{% highlight swift %}
extension Puck: Renderable
{
    func addToScene()
    {
        scene.addChild(node)
    }

    func get() -> SKNode
    {
        return node
    }

    func destroy()
    {
        node.removeFromParent()
    }
}
{% endhighlight %}

Here's the puck's implementation of the `Renderable` protocol. Granted, it is extremely simple, but it allows for complete customization on a puck-level basis. In a sense, its similar to prefabs in Unity.

Another high-level advantage of this system is that it allows for event-like components, like a `GoalScored` component:


{% highlight swift %}
protocol GoalScoredComponent
{
    func goalScored(team: String)
}

.....

extension Rink: GoalScoredComponent
{
    func goalScored(team: String)
    {
        if team == "red"
        {
            scene.backgroundColor = .red
        }
        else if team == "blue"
        {
            scene.backgroundColor = .blue
        }
    }
}
{% endhighlight %}

This, in turn, allows the code called for when a goal is scored be this:

{% highlight swift %}
func goalScored(net: Net)
{
    goalScored = true

    // find all of the GoalScored components and let them do their thing
    let score = SKAction.run({
        for c in self.entity.findAllOf(component: GoalScoredComponent.self)
        {
            c.goalScored(team: net.getTag())
        }
    })

    // find all of the GoalReset components and let them do their thing
    let reset = SKAction.run({
        for c in self.entity.findAllOf(component: GoalResetComponent.self)
        {
            c.reset()
        }
    })

    // let there be a pause between for the celebration
    let wait = SKAction.wait(forDuration: 5) //global goal celebration length

    // create a sequence from it
    let sequence = SKAction.sequence([score, wait, reset])

    // and run it! upon completion, start a new faceoff
    scene.run(sequence, completion: {
        self.goalScored = false
        self.entity.add(component: Faceoff(entity: self.entity, name: "faceoff", tag: "UI"))
    })
}
{% endhighlight %}

This code is incredibly concise, verbose, and clean. It pushes all of the logic of the objects into the objects themselves. It's not without its flaws: it won't work well for larger systems, it is hard to customize objects on a per-instance basis, and objects are authoritative over their own implementation. For _DASHockey_ though, this is a great system that allows the game to run in a concise matter.
