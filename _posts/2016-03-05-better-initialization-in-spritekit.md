---
title: "Better object initialization and setup in SpriteKit."
description: "Closures can be used to reduce class' init() function calls by moving variable initialization just a bit further up."
---

Here's a neat little tidbit that I found in the [Swift programming handbook.](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/Initialization.html#//apple_ref/doc/uid/TP40014097-CH18-ID232)

Let's say you have a Menu object:

{% highlight swift %}
class MainMenu : Menu
{
    override init()
    {   
        super.init()
    }

    //the rest of the class . . .
}
{% endhighlight %}

Now, lets say this menu has a text label and a button to start the game.

{% highlight swift %}
class MainMenu : Menu
{
    let title:Text!, button:Button!

    override init()
    {
        super.init()
    }

    //the rest of the class . . .
}
{% endhighlight %}

To initialize these values, you would have to do it before the super.init() call since they aren't optionals (think, the button is always going to be there). Usually, something similar to this:

{% highlight swift %}
class MainMenu : Menu
{
    let title:Text!, button:Button!

    override init()
    {
        title = Text(text: "This is a text node.",
                     position: CGPoint(x: Constants.stageWidth / 2,
                                       y: Constants.stageHeight),
                     fontSize: Constants.large,
                     fontColor: Constants.text,
                     fontName: "Nexa Light")
        title.name = "titleText"

        button = Button(text: "Play",
                        position: CGPoint(x: Constants.stageWidth  / 2,
                                          y: Constants.stageHeight / 4),
                        fontSize:  Constants.small,
                        fontColor: Constants.text,
                        fontName: "Nexa Light",
                        imageName: "button" + Constants.assetSize)
        button.name = "button"

        super.init()

        scene.addChild(title)
        scene.addChild(button)
    }

    //the rest of the class . . .
}
{% endhighlight %}

This isn't too cumbersome, but what if we set up these values for animating in when the scene loads?

{% highlight swift %}
class MainMenu : Menu
{
    let title:Text!, button:Button!

    override init()
    {
        title = Text(text: "This is a text node.",
                     position: CGPoint(x: Constants.stageWidth / 2,
                                       y: Constants.stageHeight),
                     fontSize: Constants.large,
                     fontColor: Constants.text,
                     fontName: "Nexa Light")
        title.name = "titleText"
        title.alpha = 0
        title.position.y += title.getHeight()


        button = Button(text: "Play",
                        position: CGPoint(x: Constants.stageWidth  / 2,
                                          y: Constants.stageHeight / 4),
                        fontSize:  Constants.small,
                        fontColor: Constants.text,
                        fontName: "Nexa Light",
                        imageName: "button" + Constants.assetSize)
        button.name = "button"
        button.scale = CGPointMake(x:0, y: 0)

        super.init()

        scene.addChild(title)
        scene.addChild(button)
    }

    //the rest of the class . . .
}
{% endhighlight %}

This, too, still isn't too bad, but most screen spaces have more than just a button and a text node, so this quickly gets out of hand, with init calls being these humongous methods that can't be broken down into smaller methods. But they can be shrunk down. We can move most of the configuration into the same space that we use for property initialization using closures:

{% highlight swift %}
class MainMenu : Menu
{
    let title:Text! =
    {
        text = Text(text: "This is a text node.",
                    position: CGPoint(x: Constants.stageWidth / 2,
                                      y: Constants.stageHeight),
                    fontSize: Constants.large,
                    fontColor: Constants.text,
                    fontName: "Nexa Light")
        text.name = "titleText"
        text.alpha = 0
        text.position.y += text.getHeight()

        return text
    }()

    let button:Button!
    {
        tempButton = Button(text: "Play",
                            position: CGPoint(x: Constants.stageWidth  / 2,
                                              y: Constants.stageHeight / 4),
                            fontSize:  Constants.small,
                            fontColor: Constants.text,
                            fontName: "Nexa Light",
                            imageName: "button" + Constants.assetSize)
        tempButton.name = "button"
        tempButton.scale = CGPointMake(x:0, y: 0)

        return tempButton
    }()

    override init()
    {        
        super.init()

        scene.addChild(title)
        scene.addChild(button)
    }

    //the rest of the class . . .
}
{% endhighlight %}

Two things to remember, though, that the Swift documentation tells us:

> Note that the closure’s end curly brace is followed by an empty pair of parentheses. This tells Swift to execute the closure immediately. If you omit these parentheses, you are trying to assign the closure itself to the property, and not the return value of the closure.

> If you use a closure to initialize a property, remember that the rest of the instance has not yet been initialized at the point that the closure is executed. This means that you cannot access any other property values from within your closure, even if those properties have default values. You also cannot use the implicit self property, or call any of the instance’s methods.

So that means we can't call any methods to the object being created as it technically isn't created yet, but we can call methods on the temporary object we are returning, which is good enough in most instances.

What's nice about this is that it makes our init functions much smaller and the code inside of it is simply initializing the class now, rather than all of the members in the class which may or may not be completely in-line with the goal of the class. This, [combined with lazy declaration](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Properties.html), are two tools that can make inits more manageable and readable and not half the size of the entire class.
