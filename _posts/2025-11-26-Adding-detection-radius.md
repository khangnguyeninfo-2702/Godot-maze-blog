---
layout: post
title: "Devlog #8: Smart Enemy Detection with Multiple Raycasts"
date: 2025-11-26 11:24:00
categories: [godot, programming, devblog]
tags: [enemy-ai, optimization, raycasts]
---

## Overview

This week's focus was on **enemy intelligence** and **shop UI improvements**. I implemented a multi-raycast detection system that makes enemies smarter while keeping performance optimized for future multiplayer support.

---

## 🎯 What I Built

### Enemy Vision System
The new detection system uses multiple raycasts spread across a configurable angle, combined with a detection radius to optimize performance.

<p align="center">
  <img src="{{ site.baseurl }}/assets/images/multiple_raycasts+detection_radius.png" alt="Multi-raycast detection system showing detection radius and raycast spread" width="800" />
  <br>
  <em>Multiple raycasts with detection radius for intelligent enemy behavior</em>
</p>

### Shop UI Redesign
Improved the shop layout using panels instead of raw texture drawing, significantly increasing visibility and visual clarity.

<p align="center">
  <img src="{{ site.baseurl }}/assets/images/better_shop_layout.jpg" alt="Redesigned shop interface with glowing panels" width="800" />
  <br>
  <em>New shop layout with panel-based UI and glow effects</em>
</p>

---

## 📚 Key Learnings

- **Dynamic Raycast Spawning**: Learned to spawn and configure multiple raycasts entirely through code
- **Property Configuration**: Set raycast properties (angle, length, collision masks) programmatically
- **Performance Optimization**: Implemented timer-based checking to avoid per-frame collision tests
- **Spatial Distribution**: Calculated equal angular distribution for any number of raycasts

---

## 🔧 Technical Challenges & Solutions

### Challenge 1: Dynamic Raycast Generation

**Problem:**
- Needed to spawn raycasts dynamically without hardcoding their count
- Wanted equal angular distribution across a configurable spread angle
- Had no prior experience modifying raycast properties via code

**Solution:**
```gdscript
# Example approach
@export var raycast_count: int = 5
@export var detection_angle: float = 90.0

func spawn_raycasts():
    var angle_step = detection_angle / max(raycast_count - 1, 1)
    for i in range(raycast_count):
        var raycast = RayCast2D.new()
        var angle = -detection_angle/2 + (i * angle_step)
        raycast.target_position = Vector2.RIGHT.rotated(deg_to_rad(angle)) * detection_range
        add_child(raycast)
```

**Key Insight:** The math was straightforward (angle ÷ raycast count), but understanding how multiple rays improve enemy "intelligence" through better field-of-view coverage was the real breakthrough.

---

### Challenge 2: Performance Optimization

**Problem:**
- Multiple enemies × multiple raycasts per enemy = potential performance bottleneck
- Checking collisions every frame would be expensive at scale

**Solution Implemented:**

1. **Detection Radius**: Raycasts only activate when the player enters a trigger area
2. **Timer-Based Checks**: Limited collision checks to once per 0.5 seconds instead of every frame

```gdscript
# Pseudocode
var check_timer: Timer
var detection_area: Area2D

func _ready():
    check_timer.timeout.connect(_check_raycasts)
    check_timer.start(0.5)
    
func _check_raycasts():
    if player_in_detection_radius:
        # Only check raycasts when player is nearby
        for raycast in raycasts:
            if raycast.is_colliding():
                handle_detection()
```

**Result:** Significant performance improvement while maintaining responsive enemy behavior.

---

### Challenge 3: Multiplayer Target Selection

**Problem:**
- Game is planned as multiplayer - which player should an enemy chase when both are visible?
- When should enemies re-evaluate their target?

**Solution:**
Discovered that raycasts return collision distance! This made target selection trivial:

```gdscript
func select_closest_player():
    var closest_player = null
    var min_distance = INF
    
    for raycast in raycasts:
        if raycast.is_colliding() and raycast.get_collider().is_in_group("players"):
            var distance = raycast.global_position.distance_to(raycast.get_collision_point())
            if distance < min_distance:
                min_distance = distance
                closest_player = raycast.get_collider()
    
    return closest_player
```

**Optimization:** Target re-evaluation happens once per 0.5s, preventing unnecessary calculations while keeping enemies responsive.

---

## 🚀 Next Steps

With the foundation of intelligent enemy vision complete, I'm moving forward with:

1. **Enemy Variety**: Creating elemental enemy types (Fire, Wind, Ghost) with unique abilities
2. **Special Mechanics**: Implementing type-specific behaviors and attacks
3. **Core Game Mechanic**: Beginning work on the main twist - **breaking the barrier between 2D and 3D worlds**

---

## 💭 Reflections

This week reinforced the importance of thinking about optimization early, even before performance problems arise. The detection radius and timer-based checking will scale well as the game grows more complex. The biggest surprise was how simple problems (like distance checking) sometimes have elegant built-in solutions - raycasts automatically provide collision distance!

---

*Thanks for reading! Stay tuned for the next devlog where I'll dive into the 2D/3D world-breaking mechanic.*

**Previous:** [Devlog #7](#) | **Next:** [Devlog #9](#)