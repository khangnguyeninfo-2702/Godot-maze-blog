---
layout: post
title: "My third blog post! Adding artillery projectiles"
date: 2025-11-08 11:07:00
categories: [godot, programming, devblog]
---

On 07/11/2025, I try adding artillery projectiles. Since I have no idea how to make this on my own, I searched on youtube and learn how to make it.
- To break it down simply, I successfully implemented this through making a sprite for my projectile, and a sprite for its shadow to sell the illusion 😅
- I also create a timer to dispose of the projectile a certain amount after it touches the ground so it doesn't look like it just immediately disappears after touching the ground. Which can be weird...
- Additionally, I create an area2d node which is the explosion radius of the projectile and just copy the code that I've written for my landmine's explosion to this one. Pretty neat.
Here is what it looks like when done!
<p align="center">
<img src="/assets/images/Artillery_projectile.jpg" alt="Player firing an artillery projectile" width="800" />
</p>
---
What I learned:
- Sometimes, already written codes comes in handy in times like this so having a clean code set up is crucial to finding where the codes are and knowing what they do to duplicate or modify them easily.
- The math behind the projectile trajectory of an artillery...
---
Troubles encountered:
- Understanding the math behind the projectile's trajectory but that's not related to the game so I just spend quite some time studying it.
- Choosing the direction in which the player shoot the projectile.
- To solve the latter problem, I add a simple direction changing mechanism into the input map of the project settings. pressing up arrow means direction is up and so on. This limits the direction the player can actually shoot the projectile but that's what I wanted otherwise the projectile would just be OP.🥲
