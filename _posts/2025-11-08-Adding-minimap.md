---
layout: post
title: "My forth blog post: Creating my first minimap!"
date: 2025-11-08
categories: [godot, programming, devblog]
---

On 2025/11/08, I decided that my screen was looking a little bit plain and my maze will eventually grow to be out of the screen once a player reaches a high level.
- This is when I decided to create a minimap on the top right corner of the screen, which is, for now, is just simply a zoomed out version of the actual map.
---
What I learned:
- How to use viewport/line nodes to create a cool border.
Here is an image of how it turns out, pretty simple looking but I sure do ahve some trouble figuring out how to properly make it 😅
<p align="center">
<img src="/assets/images/Add_minimap.jpg" alt="An image of a simple minimap in the top right corner of the game" width="800" />
</p>
---
First problem encounter:
- Getting the border to surround the minimap view
- When I first create the border, I put it as a child of the minimap and so when I run, it appears to be somewhere that's not fixed and keeps moving around in the minimap.
- It just didn't work like expected, despite the fact that I saw videos online that do that and the border surrounds the minimap as expected. Mine wasn't that way.
- To fix this, I had to put the border as the child of the canvaslayer, not the actual minimap and that's it.
Second problem encounter:
- Getting the minimap to actually the map the player is on.
- When creating a subviewport, I needed to give it a world. In this case a world2D variable. I couldn't see where the world2D variable was.
- I couldn't see it in the root node as well and videos on youtube again, didn't help as it just works for them. Not for me. Maybe I'm the problem...
- Eventually, I found the solution is just to access the variable world_2d in the script since I couldn't find that in the inspector but it works and this sure is noted down.
