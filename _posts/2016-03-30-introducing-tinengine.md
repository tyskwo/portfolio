---
title: "Introducing TinEngine."
description: "Over my (short) Swift and SpriteKit excursion, I've compiled a list of extensions and functions that have made my life a little easier."
---

I've been working in SpriteKit a lot lately, and one of the many beauties of Swift is the capability of extensions. As I've been working on these prototypes of mine, there have been reoccurring patterns of how I've done certain tasks, and I've conglomerated all of these into what I call TinEngine. Now yes, the name is misleading, because SpriteKit itself isn't even an engine – it is a framework, and TinEngine is simply an extension of this framework, so something like TinExtensions is much more accurate, but regardless, TinEngine, as of now, fills three major caveats that I think are present in SpriteKit.

The naming of it comes down to the idea of tin being a lightweight and malleable metal, and that's how I want these extensions to be used – as easier ways to perform already-implemented functions (for the most part). These extensions are also independent of each other, and individually are pretty small.

The first of these extensions is a re-working of the SKScene and putting it more in-line with FlashPunk and similar stage-centric engines. The transitions between SKScenes are lacking, and aren't very customizable outside of the default options, with some people resorting to writing custom shaders imported into CAImageFilters to transition between scenes, but these are an all-or-nothing solution and make it hard to transition elements in and out individually. Tin takes the first SKScene and creates a 'stage' reference to it. There are then TinScenes, which have transitionIn() and transitionOut() functions, allowing for more control when moving to and from different sections of the game: like from the main menu to the gameplay for instance.

The slightly larger extensions of SpriteKit is its motion system, which builds on top of SKActions, incorporating all the tweening functions you've come to know and love, and also the ability to group and sequence these tweens in fewer lines of code than it would take using straight SKActions. Right now, fade, rotate, scale, and move are supported, but I hope to add more as time goes on.

Lastly, and possibly least importantly, I've collected all the extensions I've used with basic data types and put them in one place – things like CGPoint to vector conversions, clamping, randomization (which is slightly out of date since the introduction of GameplayKit), and rounding.

That's it for now for TinEngine, but I hope to add more as I stumble across more problems in my SpriteKit journey.

The [repo is available here](https://bitbucket.org/tyskwo/tinengine/overview"), feel free to download, use, or redistribute. The license for it is [CC-BY 3.0.](https://creativecommons.org/licenses/by/3.0/us/legalcode")
