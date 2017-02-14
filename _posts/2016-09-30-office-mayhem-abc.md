---
title: "Office Mayhem: A + B = C"
description: "Or more concisely, how to create a modular system that allows for emergent gameplay."
---

[_Office Mayhem_](http://tyskwo.com/2016/09/25/dive-dive-dive/) is [my](http://willconcannonart.com) [team's](https://jeremyroot.squarespace.com) [capstone project,](http://tyskwo.com/2016/09/19/the-beginning-of-the-end/) where you fight for 'Employee of the Day' by completing tasks and sabotaging your opponents! The quality of the gameplay rests on the shoulders of a modular, interactive, and emergent item interaction system. That's what I've been working on the past week.

<figure class="align-center" style="width: 640px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/09/exampletask.gif" alt="">
  <figcaption>Here, we're picking up paper, copying it, creating a report, and sending it out. Then we pick up the outbox, toss it on the floor, create another report, and send that out too.</figcaption>
</figure>

In order to achieve this level of modularity, we really needed to understand the kinds of interactions that we want the player to be able to perform. For now, we have these item components broken down into six categories:

`Actionable, Pickupable, Sabotagable, Holder, Creator, Completor`

These are umbrella-ed under an `Item` class which handles the interactions between these different modules. For instance, a report is `Pickupable`, whereas an outbox is `Pickupable`, `Actionable`, and a `Completor`.

<figure class="align-left" style="width: 300px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/09/pickupsabotage.gif" alt="">
  <figcaption>This is the kind of emergent gameplay we're hoping for: you can sabotage a computer, and rip it off the desk it's on, and then do whatever you please with it.</figcaption>
</figure>

In general, all items that are `Completor` or `Creator` are also `Actionable`, since they are being used to create an item/complete a task. `Sabotagable` items have a flag on them on whether or not they can be picked up when sabotaged. Obviously, a huge industrial copier can't be picked up, but a computer definitely can be. This introduces a two-stage sabotage system: players can simply sabotage items, or they can sabotage them on toss them in a corner somewhere. We want to promote sabotaging as a way to get ahead, rather than just a thing as convenience. Similarly, there are items that can be `Actionable` even though they aren't currently placed on a `Holder`, like an outbox. This system is meant to be used with prefabs, so all items of the same type would act the same, lending an element of predictability for the player. For this to be truly modular though, I needed to create a system that would make it easy for the designers to create new relationships for item creation, destruction, and task completion.

I set out to create a suite of tools that can be used in the Unity editor to achieve this. I wanted to symbolize these relationships in an easy to understand fashion that would be instantly recognizable and understandable. I came up with the idea of representing them through the simple equation of A + B = C, or more descriptively, Used Item + Held Item = Created Item.

<figure class="align-right" style="width: 300px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/09/relationship_creation_tool.gif" alt="">
  <figcaption>Here's the tool in action. You can add new relationships, change and delete previous ones, and add them to the game, all from the editor.</figcaption>
</figure>

For the example at the top of this post, the used item is the copier, the held item is the paper, and the created item is another paper. Since the player can only hold one item at a time, and use one item at a time, there will only be these A + B relationships. This could be easily changed later on though, to have it be A + B + C = D, etc.

Being a part of the editor, it is really easy to access and change the relationships in the game, rather than having to do it through code. This allows the designers to test different relationships easily, to explore different concepts. For example, should the player be able to copy a sabotaged computer? Or what about throwing one out? It allows for rapid creation and testing. Similar to this item creation tool, I have an item destruction one as well. For instance, if the player uses a garbage pail while holding a report, the report gets destroyed. The equation for this relationship is A ->Destroys-> B, where A is the used item, and B is the held item. That tool is also implemented, complete with keyboard shortcuts.

<figure class="align-right" style="width: 300px">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/blog/2016/09/proof.gif" alt="">
  <figcaption>And here's proof that it really is as simple as A + B = C.</figcaption>
</figure>

I also want to create a tool that simplifies task completions, similar in the A + B = C format, where C is now the task completed. For this to be started though, I need to figure out how I'll be representing tasks on the backend – probably similar to how I handle items. Once that is complete though, the tool creation won't be hard since it will be similar to the first two. This suite as a whole will allow the designers to dictate what relationships create items, what relationships destroy items, and what relationships create tasks. This, combined with the individual components attached to the prefabs, allow the designers to have complete control over the interactions of the world. At least until they ask for a new type of item.

I'll be writing up a blog post on using Unity's Editor extensions over the weekend, it won't be long as this – it's surprisingly easy! Next up is creating the task system, and then after that is the player input actions system. Lots to do, and little time to do it!

Along with my perspective from the technical side of the game, go follow [Jeremy](https://jeremyroot.squarespace.com) and [Will](http://willconcannonart.com) for design insights and awesome pictures of art!
