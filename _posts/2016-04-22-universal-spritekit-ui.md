---
title: "Universal SpriteKit UI"
description: "Apple's mobile devices are split into two aspect ratios, but there are ways to create a singular UI using the viewport to your advantage."
---

Ignoring the almost defunct iPhone 4s, Apple's mobile devices come in one of two aspect ratios: 16:9 for iPhones and 4:3 for iPads. Generally, if a game is simple enough, it is best practice to create the SKScene to be the same size of the viewport at the expense of some additional whitespace when running the game on iPads. If the game has specific levels that require more complex scenes through the scene editor, they won't be the exact size of the viewport, and very often an SKCameraNode will be used. In these instances, it would be difficult to get the exact size of the screen without taking into account the scene size. When researching solutions, I stumbled upon [this great and answer on StackOverflow.](http://stackoverflow.com/questions/29161881/skspritenode-position-in-universal-game)

Basically, that convert function converts screen coordinates into view coordinates, which is great for placement of UI when dealing with different aspect ratios (or entire games if they don't require complex scenes).
