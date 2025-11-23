---
layout: post
title: "Devlog #6: The Hidden Agony of Health Bars (and How I Solved It)"
date: 2025-11-16 09:24:00
categories: [godot, programming, devblog, ui]
---

## 📅 The Challenge: Health Bars 😫

On November 16th, I successfully implemented health bars for both enemies and the player. I thought this feature would be trivial, especially since my **Health Component** script already handles all the damage and HP tracking.

I was dead wrong. What I expected to be a simple one-hour task turned into a full day of suffering and debugging. This experience proved that even the simplest-looking UI elements can hide complex mathematical and structural problems.

---

## 🎨 The Result

Here is the result of the struggle. The health bars now correctly follow the player and enemies, **maintain proper visual offsets** even when the character's size changes, and **change color** dynamically based on the health remaining.

<p align="center">
<img src="{{ site.baseurl }}/assets/images/Add_health_bars.png" alt="player and enemies with health bars" width="800" />
</p>

---

## 🧠 What I Learned & Problem Breakdown

This whole process boiled down to solving two core problems related to UI positioning and scaling in a dynamic 2D environment.

### Problem 1: Generalizing the Health Bar Offset

The first major hurdle was ensuring the health bar **followed the target accurately** regardless of their size.

* **The Difficulty:** In my game, the player and enemies dynamically change size. I needed a universal way to position the health bar **above** the entity that wouldn't break when the entity scaled up or down. A fixed, arbitrary offset was insufficient.
* **The Solution:** I found a way to generalize the offset by treating the position dynamically. The key was calculating the required offset by using the **height of the entity's collision shape** and the **height of the health bar itself**. This created a robust, generalized formula that anchors the health bar properly above the entity's bounding box, ensuring the distance remains consistent visually.
    * **Learned:** How to make UI elements keep a certain distance from dynamic objects and controlling offsets using relative sizes.

---

### Problem 2: Debugging Size-Dependent Scaling

The second problem involved correctly making the health bar's width **scale proportionally** with the width of the player or enemy.

* **The Difficulty:** This should have been an easy one-liner, but my scaling mechanism kept breaking.
* **The Breakthrough:** After hours of frustration, I realized the issue wasn't the scaling code itself, but a logic flaw in my component initialization. The player's size was being updated only once **at the moment of creation**. This timing issue meant a crucial boolean flag that checked if the size had been initialized was the **opposite** of what I expected it to be, effectively breaking the scaling mechanism from the start.
* **The Fix:** A simple refactoring of the initialization order and ensuring the sizing logic runs *after* the initial size calculation fixed the issue immediately. Sometimes, the hardest bugs are the ones stemming from simple timing and logic errors!

---

## 🎬 Next Steps

With robust health bars in place, the core gameplay loop feels much more complete. Next up, I may add some extra armor health bars to player in case when they can pick up armors or weapons later on 😎
