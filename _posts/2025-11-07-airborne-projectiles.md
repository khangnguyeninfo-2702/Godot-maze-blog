---
layout: post
title: "Devlog #3: Adding Artillery — Mastering the Cinematic Arc"
date: 2025-11-08 11:07:00
categories: [godot, programming, devblog, projectiles, physics]
---

## 🎯 The Feature: High-Arc Artillery

On November 7th, I integrated artillery projectiles into the game. Since I hadn't implemented parabolic motion before, I turned to online resources to learn the physics and math necessary for a satisfying high-arc shot.

### The Illusion of Altitude

The key to selling the artillery effect in a 2D game is creating the **illusion of 3D depth and altitude**. I achieved this with two main components:

1.  **The Projectile Sprite:** This is the main asset that follows the parabolic path.
2.  **The Shadow Sprite:** Crucially, a separate sprite follows a flattened version of the trajectory along the ground plane.

This setup ensures that as the projectile arcs high into the air, its shadow remains below it, giving the player a visual cue for where the projectile will land and making the whole effect feel much more cinematic.

### Reusing the Boom 💥

One huge benefit of my modular component design came into play here. My existing **landmine explosion logic** was easily duplicated and adapted for the artillery shell:

* I created an **`Area2D`** node to define the explosion radius.
* The code that handles checking for enemies/walls within that radius and applying damage was simply copied over from the landmine script. Clean code setup really paid off!

<p align="center">
<img src="{{ site.baseurl }}/assets/images/Artillery_projectile.jpg" alt="Player firing an artillery projectile" width="800" />
</p>

---

## 🧠 Lessons Learned

This feature taught me two major things about project efficiency and game design:

* **Code Reusability is King:** Having well-defined, self-contained scripts for effects (like an "Explosion Component" or damage calculation) is crucial. It speeds up development immensely when you can reuse large blocks of logic for new features.
* **The Math Behind the Trajectory:** I spent time studying the trigonometry and physics required to calculate the projectile's trajectory, which, while not a core Godot feature, is fundamental game development knowledge.

---

## ⚙️ Development Challenges

### Problem 1: Choosing the Firing Direction

Initially, I had a hard time deciding how to control the direction of the artillery fire. Free-aiming would make the projectile too powerful (OP).

* **Solution:** I implemented a simple, restrictive firing mechanism linked directly to the player's movement input. Pressing the **Up arrow** sets the firing direction to Up, **Right arrow** sets it to Right, and so on. This limitation creates a challenging but balanced mechanic that requires the player to position themselves correctly before firing.

### Problem 2: Timing the Disappearance

When the projectile finished its flight path and hit the target area, it instantly disappeared. This looked visually jarring and unprofessional.

* **Solution:** I implemented a **simple timer** on the projectile node. Instead of immediately queuing the projectile for deletion when it hits the ground, the timer starts. When the timer runs out a short time later, the projectile is disposed of. This creates a natural, delayed "fizzle out" that improves the visual quality.

---

## 🎬 Next Steps

With the artillery system in place, I feel like I've got a strong array of player abilities. I'll be moving on to the more polish and management aspects of the game, like creating the dedicated shop system and integrating the health bar UI I'll be working on soon!