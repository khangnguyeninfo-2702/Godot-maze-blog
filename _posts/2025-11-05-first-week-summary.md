---
layout: post
title: "My First Blog Post: Getting Started with Godot"
date: 2025-11-05 09:24:00
categories: [godot, programming, new]
---

In today's blog I will summarize what I have learnt and done through 1 week of programming in Godot.

The first day:
2025-10-29
Starting up Godot, I start learning about the syntax and the actual language GDScript, it is actually very similar with Python and I have had some experience with Python so it didn't take long to learn the actual language.
After that, I get straight into creating a maze like game since I was learning about maze creation/solving algorithms at the time. I implemented finding neighbors function and then the actual DFS algorithm.
After drawing the maze, I have a lil bit understanding on how to use the draw related methods in godot.
Then I created the collision for the maze by creating a new scene with a static body and collision shape and instantiate that whenever a cell is drawn.

Troubles I had:
- None since I am still using my previous python knowledge to code this algorithm 😎
- Creating rectangles and store store the walls/paths position in a grid through symbols and iterate through the grid to correctly instantiate the collision blocks at the right place.

The second day:
2025-10-28
On this day, I started using actual Godot's functions. I first try to visualize my maze onto the game screen, I have also implemented this previously in python and turns out GDScript is also quite similar
with python in drawing the maze. Then the maze was visually shown on the screen, I noticed a problem is if the maze gets too big then it will go out of the screen and it won't be fully visible anymore so
I implemented panning and zooming through a Camera2D node.

Troubles I had:
- Implementing zooming and panning was actually quite simple but the logic of limiting the zoom and getting the mouse's direction was not so simple as I just learned about this language...

The third day:
2025-10-30
It is this day that I started to think about actually making a game out of what I learn through creating a maze. But creating a maze and solving it just seems boring so I started thinking of things that
can be done to increase the game's "enjoyability" (Don't know if that's a real word or not). 

So with this idea in head of making a maze related game, I started relocating scripts and scenes nicely for future folder locating purposes. Then I started learning about Godot's built in nodes, specifically 
StaticBody2D and CollisionShape2D nodes. I then create a Node2D and turn it into a cell shape with collision.
At this point, I learn how to instantiate other scenes into a scene and successfully created a maze with bunch of collision shapes.
Then I created the player, I learn how to handle and map inputs in project settings from the keyboard. I also created orbs that can change the player's color and properties like increasing speed, size or reducing
speed, size.
Troubles I had:
- Creating collision shapes, that matches the coordinates of the actual drawn cell, I need to create 2 grid, 1 for the in grid coordinates and 1 for the on screen coordinates.
- Without the second grid, all the wall tiles would just spawn pixels away from each other.
- I need to match the collision shape size to the cell size automatically, at this point I learn how to access other scenes' variables from a scene's script.
- Finding a valid spawn point on the screen for players and orbs. I created a function to get the "center" location of every cell in the grid on screen and create a function to spawn orbs randomly among those
coordinates. Once an orb is spawned that coordinate of the orb is removed to ensure the player/ other orbs spawn at the same place.

The 4th day:
2025-10-31
Today, I thought of a big twist that I can add to the game which has the potential to become the main attraction of my game. In order to prepare for that, I need a node that takes care of the whole game's state.
This is when I know about singletons or Autoload objects. Basically nodes that never gets deleted as the game progress, great for tracking the player's data.

After connecting the game's state to the GameManager Node, I added test effects on the orbs to see how it interacts with the player and then I started working on the first real ability in my game "JUGGERNAUT" 💪.
This juggernaut ability allows the player to move through walls and destroys them which means basically destroying the collision shapes of the wall at this point.

First problem encountered:
- The maze will be completely destroy if the player runs around the maze with the juggernaut ability on making all collision shapes disappears. To solve this, I implemented a wall healing system, basically tracking the coordinates of every wall destroyed in a list and then spawn back a wall at the same place with collision shape after the ability runs out.
- In order to do that, I must use a Timer so I learned how to use timers and add them to the player's only ability at this point and also added timers for healing the wall.
Second problem encountered:
- When the walls spawn back but the player is still at a place that a wall was destroyed, the player will become stuck in collision blocks.
- To fix this, I used the previously mentioned grid of all valid spawn points for the orbs/players.
- When a wall spawns back, it will check at its position if the player is in its radius, if yes then the game will teleport the player to the closest valid paths in the maze which are not walls

 The 5th day:
 2025-11-01
 I continue adding abilities to the orbs. This day, the ability added was "GHOST" 👻.
 The ghost ability allows the player to phase through walls and increases the player's speed at the same time.
 In order to do this, I do the steps I learn through creating the first "JUGGERNAUT" ability:
 
 Step 1: Create ability-related variables (Ex: If ability changes player's color then create a new color variable)
 Step 2: Create a timer
 Step 3: Create the ability duration
 Step 4: Add the timer as the child of the current scene.
 Step 5: Start the timer when player picks up orbs.
 Step 6: Connect timer's timeout signal to a _on_timeout_function.
 Step 7: Reverse the player's properties to the original state in the timeout function

 To immplement this ghost ability, I set the player's collision mask of 1 to false since the walls are now currently on layer 1. However, I also want to add some visual change apart from the player's color so when the ability is active, I added a trail to the player which matches the player's current color.
 I also added enemies with no pathfinding at all, but the enemies have line of sight which is a raycast that always point at the player and if the raycast is not colliding with any cells then the enemies will start heading towards the player.
 Otherwise, the enemy will in patrol state which is just moving in a direction and randomly changes direction if it collides with a wall.
 
 What I learned:
 - After adding "GHOST" ability, I have some understanding of Timer and Signals and Collision processings.
 - After adding enemies, I know about basic enemy works and enums.
 - After fixing the destroying/phasing through outerwalls (supposedly walls that are designed to keep players inside the maze). I know briefly how to use groups in Godot.

First problem encountered:
- I am allowing the player to have multiple abilities at once so if the player picks up 2 abilities and the first ability runs out then the player will be revert back to normal state even when the second ability is still active.
- In order to solve this problem, I added a system in GameManager Node to know which abilities are active and their orders to deactivate them correspondingly and only fully revert the player back to original state when all abilities ran out.
Second problem ecountered:
- When the ghost ability runs out and the player is still in a cell then the player will then also be stuck. This problem sounds at first similar to the Juggernaut ability but it's actually a bit different and easier to solve.
- In order to resolve this, in the timeout function, I use the test_move built in function to check at that moment is the player touching any cells then the player will be tp back to the neareast safe point.
Third problemm encountered:
- When first adding enemies, they can also collect orbs and those effects will be applied on the player...
- To resolve this, I fix the collision and mask layers of the orbs and enemies and players.
Forth problem encountered:
- When ghost or juggernaut ability is active, the player actually can smash through the outer wall that I explicitly created to make the maze looks prettier.
- To fix this and track collisions, I added players to player_group, enemies to enemy_group, inner walls and outer wall to their own groups.
- I put the outerwall on a different layer and only when the player collides with an inner wall group object does the wall explodes.

The 6th day:
2025-11-02
Today, I add a new ability called "GODSPEED" inspired from an anime. This ability increases the player's speed and allows player to place landmines which flickers after 2.5 secs and explodes after 5 seconds.
To do this, I created a new scnene named landmine, created its shape, triggered radius and explosion radius.
I mapped a new place landmine input to the project so when player press "p" then a landmine will be placed.
And I followed the aforementioned steps to create abilities that I learned on the fifth day to also create this "GODSPEED" ability.

What I learned:
- Adding multiple collision shapes to a single object with different functions.
- Using time interval to add the flicker effect.
- Using GPUParticles2D to create explosion particles

First problem encountered:
- Adding the flicker effect to the landmine.
- In order to solve this problem, I created a flickering state and a time interval Timer that decreases over every flicker. On this interval Timer's timeout, the landmine color will be changed.
Second problem encountered:
- I didn't know how the game would work if multiple collision shapes was added onto a single object.
- This problem is not exactly hard to solve but it was weird to me that multiple collision shapes can be added so it took me sometime to get this to work right, the explosion radius just doesn't damage the enemy when I created it at first.
Third problem encountered:
- Limiting bombs placed and the FIFO storage of bombs.
- I wanted it so that player can only place 4 bombs, there's no limit to how often a player can place bombs but when the fifth bomb is placed the the oldest bomb will be destroyed, not exploded.
- In order to solve this, I had to implement a FIFO storage system of the landmine scene and track how many bombs have been placed through a bomb placed signal in the landmine script.

The 7th day:
2025-11-03
On this date, I started noticing that the game is looking good but there is no sound yet which makes it lifeless to me.
So I decided to make some noise 📢
I went onto a royalty free website that is full of SFX that I needed.
After choosing all the SFX needed, I converted them to .wav format cause it's better that's way with short SFX.
Then, I created a SoundManager Autoload object similar to the GameManager but this one takes care of sounds.
I also needed specific sounds to play at some instance and some other sounds has to be muted when something is happening.

What I learned:
- Using buses to divide the sounds into different categories like background music, voice or SFX.
- Using AudioStream and AudioStreamPlayer and Arrays to store tons of SFX for 1 action to make the game more diversed.

First problem encountered:
- Certain sounds play over other sounds and it becomes a mess really quickly.
- To fix this, I created an array of audiostreamplayer and limits its size to 5 so only at max 5 SFX can be played at the same time.
Second problem encountered:
- When the win/lose music plays, I want all other sounds to be muted so the player knows which state the game is in.
- In order to do this, I used buses, and divided the sounds to different buses and mute certain buses when a sound is being played.

And that's the end of a week of learning how Godot works.
