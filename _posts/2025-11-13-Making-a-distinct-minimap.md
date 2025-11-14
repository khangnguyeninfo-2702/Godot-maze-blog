---
layout: post
title: "My fifth blog post: Creating a distinct minimap"
date: 2025-11-13 12:00:00
categories: [godot, programming, new]
---
The past few days, I have been adjusting the map and changing it so that the minimap has different visual than the actual real map. The minimap visuals will be simpler than the main game visuals.
---
Here is what my game looks like as of today, the player on the minimap has a little bit of glow (barely noticeable). 
<p align="center">
<img src="/assets/images/minimap.jpg" alt="Cavern theme minimap, distinct visualization minimap" width="800" />
</p>
What I learned:
- Using shaders to create glow.
- Using WorldEnvironment to create glow
- How to use a subviewport to support a minimap view.
- Spawning in player and enemies sprites on minimap and make them follow the real coordinate of the visual in the main game.

---
First problem encountered:
- Actually making the minimap distinct from the game map.
- I had this problem since I couldn't find any World2D property in the inspector dock of the subviewport node and therefore I used the main game world as the world for the subviewport through script.
- But doing this results in the stuffs that are belong to the minimap are also visible on the main map.
- To fix this, all I had to do was to create a new World2D for the subviewport. But believe me 😞, it took me 2 days to figure this out...This is the biggest problem I've encountered since the day I started making this game.
---
Second problem encountered:
- Uncertainty on the type of map to use.
- While trying to make the minimap looks like what I want, I kept on changing back and forth between drawing an image at a corner of the screen and that can be a minimap or actually using a subviewport as a minimap.
- The latter approach is absolutely 10.000.000.000 times better, I didn't even need to calculate the coordinates of the player since the camera follows the player, there is no need to scale down the map and do anything that is related to scaling down.
