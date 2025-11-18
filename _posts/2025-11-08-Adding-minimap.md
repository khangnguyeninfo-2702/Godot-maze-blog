---
layout: post
title: "Devlog #4: Creating My First Minimap — Conquering Godot Viewports"
date: 2025-11-08
categories: [godot, programming, devblog, ui, rendering]
---

## 🗺️ Why I Needed a Minimap

On November 8th, I decided to tackle a vital feature for any exploration game: the minimap. As my procedurally generated mazes grow with the player's level, the map quickly exceeds the screen bounds. A simple, static **minimap in the top right corner** was the next logical step for keeping the player oriented. For now, it's just a zoomed-out version of the main map.

<p align="center">
<img src="{{ site.baseurl }}/assets/images/Add_minimap.jpg" alt="An image of a simple minimap in the top right corner of the game" width="800" />
</p>

---

## 💡 The Technology: Viewports and Canvas Layers

Creating a minimap isn't just about shrinking the camera; it requires a deeper understanding of how Godot handles rendering the game world. The core concept is using a **SubViewport** to render a small, separate view of the scene, and then displaying that view back onto the screen using a **ViewportContainer** (or similar control node).

### Lessons Learned
* **SubViewports:** They are tiny cameras rendering a separate scene.
* **Line2D:** Learning how to use line nodes and simple `ColorRect`s to create a cool, custom border around the rendered map.

---

## ⚙️ Development Challenges and Solutions

Despite the minimap looking simple, I ran into two major roadblocks related to positioning and rendering context.

### Problem 1: The Floating Border

My first attempt at adding a border around the minimap was a disaster. I tried making the border (a `Line2D` node) a child of the minimap itself.

* **The Unexpected Behavior:** When the game ran, the border was not fixed to the screen's corner. It seemed to move and shift wildly within the minimap view, which was confusing since I expected it to be a static UI element.
* **The Core Issue:** When a node is a child of a `Viewport`, it is rendered *within* that viewport's world. If the minimap's camera moves, the border would move relative to that camera, not stay fixed on the screen.
* **The Fix:** I realized the border needs to be an **overlay** that is fixed to the screen. The solution was to place the border node as a child of a **`CanvasLayer`**. A `CanvasLayer` is crucial for UI, as it renders its children on a separate, non-scrolling layer that is always drawn on top of the main game world. Once I put the border there, it snapped perfectly into place in the top-right corner.

### Problem 2: The Missing World

To get a `SubViewport` to show the main map, you need to tell it which `World2D` resource to render. I couldn't find the `World2D` variable in the inspector of the root node, and tutorials weren't helping because for them, it seemed to just work!

* **The Difficulty:** I spent too much time hunting for a visible setting or property in the editor when the solution was simple, but hidden.
* **The Breakthrough:** The key was realizing that the `World2D` resource is automatically created and attached to the root node (usually the `Node2D` or `Main` scene) but isn't always exposed in the Inspector by default.
* **The Fix:** The solution was to stop searching the UI and simply **access the variable directly via script**: I needed to assign the root scene's `world_2d` variable to the minimap's `SubViewport` in code. This explicitly told the minimap's tiny camera, "Look at the same world the main camera is looking at," and the map immediately appeared as intended.

---

## 🎬 Next Steps

With the minimap now functional and providing crucial information, I can confidently move on to more localized UI features. The next focus is implementing a robust **shop system** to handle currency and item purchases.