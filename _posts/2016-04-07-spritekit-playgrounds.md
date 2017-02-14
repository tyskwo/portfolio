---
title: "SpriteKit + Playgrounds"
description: "Playgrounds are a great way to experiment with small prototypes and mechanics. Best part? SpriteKit can be accessed within them."
---

With Swift, Apple introduced the wonderful concept of Playgrounds: little sandboxes of code that can be run independently from a project. They're lightweight, small, and are able to show the results of your code live, line-by-line. This is great for simple experiments like learning about closures, tuples, or functions as variable types, but can be somewhat lacking when it comes to more media-rich forms of programming – like SpriteKit. Thankfully, Apple, too, has thought of this: enter `XCPlayground`.

To set up a simple Playground with SpriteKit, copy and paste this code into a fresh Playground:

{% highlight swift %}
//obviously SpriteKit is necessary,
//but XCPlayground is a module that allows
//SpriteKit to be viewed in the Assistant Editor
import SpriteKit
import XCPlayground

//Create dimensions (and find the center point)
let frame       = CGRect(x: 0, y: 0, width: 480, height: 320)
let centerPoint = CGPoint(x: frame.size.width / 2.0,
                          y: frame.size.height / 2.0)

//Create a scene
var scene = SKScene(size: frame.size)

//Create a simple sprite to add to our scene
let simpleSprite          = SKSpriteNode(imageNamed: "circle")
    simpleSprite.position = centerPoint

scene.addChild(simpleSprite)

//Set up the view and show the scene
let view = SKView(frame: frame)
view.presentScene(scene)

//this line is where the magic happens
XCPlaygroundPage.currentPage.liveView = view
{% endhighlight %}

For this to work, it is necessary to have the Assistant Editor open (Cmd+Option+Enter, or the little double-circle icon in the toolbar). What's really cool is that you can apply shaders, perform physics, run actions – anything you can do in a normal SpriteKit project. The only unknown for me is if it can detect touches through the mouse – that's next on my to-do list – but I'm assuming not. If my hunch is right, there are still some things that are better left to a full project, but Playgrounds are now a much more powerful resource for trying out (simple) new things with SpriteKit – such as experimenting with `SKConstraints`.
