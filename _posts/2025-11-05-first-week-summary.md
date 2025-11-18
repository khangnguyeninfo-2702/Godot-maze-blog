---
layout: post
title: "Devlog #1: Week One with Godot — From Maze Generation to Juggernaut Abilities"
date: 2025-11-05 09:24:00
categories: [godot, programming, devblog, fundamentals]
---

## 🚀 The First Week: Getting Acquainted with Godot

This first week (October 29th – November 3rd) was a deep dive into **Godot Engine** and **GDScript**. Since GDScript is very similar to Python, the language itself wasn't a major hurdle, allowing me to immediately focus on game structure and engine concepts.

My initial goal was to build a maze-based game, which quickly evolved into a dynamic action game focused on movement and abilities.

---

## 🏗️ Phase 1: World and Camera Foundation

The first few days were dedicated to building a visual and structural foundation:

### Maze Generation & Collision
I started by implementing the **Depth-First Search (DFS) algorithm** to generate a perfect maze. The main challenge here was bridging the gap between the algorithm's abstract grid data and the physical game world:

* **The Problem:** Translating the abstract grid coordinates into accurate on-screen coordinates for the wall tiles.
* **The Solution:** I had to implement two separate coordinate systems: one for the logical grid structure and one for the on-screen pixel positions. This was critical for ensuring all wall `StaticBody2D` and `CollisionShape2D` nodes spawned perfectly adjacent to each other, preventing tiny gaps.

### Camera Control (Zooming and Panning)
To ensure the maze was always fully visible, regardless of size, I integrated a `Camera2D` node.

* **The Challenge:** While the basic functionality was simple, figuring out the logic for **smoothly limiting the zoom** and handling mouse movement for **panning** required getting familiar with Godot's input handling and vector math.

---

## 💡 Phase 2: Game Logic and Core Mechanics

By the third day, I transitioned from a maze solver into a game developer, focusing on features that would add "enjoyability."

### The Player and Global State
I created the player character, handling keyboard inputs and the pickup of orbs that grant temporary stat boosts (speed, size, etc.). The most important decision was implementing a **Singleton (Autoload) node** called `GameManager`.

* **The Lesson:** Autoloads are essential for tracking persistent data like the player's total stats or abilities, as they never get deleted during scene changes.

### Introducing Abilities
The biggest breakthrough was adding **"Juggernaut"** and **"Ghost"** abilities, which fundamentally alter gameplay.

| Ability | Effect | Mechanism Learned |
| :--- | :--- | :--- |
| **Juggernaut** 💪 | Move through and destroy walls. | **Wall Healing:** Implementing a timer-based system to track destroyed walls by their grid coordinates and rebuild them after the ability expires. |
| **Ghost** 👻 | Phase through walls and gain speed. | **Collision Management:** Setting the player's collision mask to `false` for the wall layer (Layer 1) during the ability duration. |
| **Godspeed** ⚡ | Extreme speed boost and ability to place landmines. | **Complex Timers & Particles:** Using chained timers for flicker effects and implementing `GPUParticles2D` for the explosion visual. |

### Major Design Challenges & Solutions

1.  **Stuck Player Fix:** When the Juggernaut or Ghost ability runs out, the player can be stuck inside a newly healed wall.
    * **Solution:** Before a wall heals, or right after an ability ends, the code uses the built-in `test_move()` function. If a collision is detected, the player is instantly teleported to the closest valid (safe) path cell.
2.  **Ability Stacking:** Allowing the player to have multiple abilities active at once.
    * **Solution:** I implemented a system in the `GameManager` to track all active abilities and their remaining durations, ensuring the player's properties are only fully reverted to the normal state when **all** active abilities have timed out.
3.  **Outer Wall Immunity:** Preventing powerful abilities from destroying the *outer* boundary walls of the map, which are needed for aesthetic structure.
    * **Solution:** I organized the walls into **Groups** (`inner_wall_group`, `outer_wall_group`) and used different **Collision Layers** to control which abilities could interact with which type of wall.

---

## 🔊 Phase 3: Enemy AI and Audio (Nov 1st - 3rd)

### Basic Enemy Movement
I added enemies with rudimentary sight and patrol behavior:
* Enemies use a **`RayCast`** for their line of sight. If the ray hits the player without hitting any walls, they pursue.
* Otherwise, they enter a simple **patrol state** (moving in one direction and changing direction upon wall collision).

### Making Some Noise 📢
The final task of the week was injecting life into the game via sound.

* **The Setup:** I created an `AudioStreamPlayer` based **SoundManager Autoload** to handle all SFX and music.
* **The Learning:** I learned about **Audio Buses** to divide sounds (SFX, Music, UI), allowing me to selectively mute categories (e.g., muting all SFX when the Win/Lose music plays). I also created an array of `AudioStreamPlayer` nodes and limited its size to prevent audio clipping and noise when too many actions happen at once.

---

## 📈 Looking Ahead

I can't believe how much ground I covered in a single week using Godot! I'm moving from purely structural tasks to UI and polish. Next week, I'll be detailing the implementation of **health bars** and how I overcame the tricky positioning problems they presented.