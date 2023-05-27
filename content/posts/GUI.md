---
title: "GUI"
date: 2023-05-26T07:31:12-05:00
draft: true
---

The *graphics stack* is not very high. You can bootstrap your way from drawing primitives to widget-full interfaces in no time.

There are multiple ways of looking at the GUI.

## Model View Controller



## Scene Transformer

Every tick, the program checks for differences between the virtual DOM and what's on the screen, refreshing the graphics if necessary. This simplifies the interaction between model and view because the scene graph is a simple function of the state, which is just diffed each frame to calculate updates.

## Immediate Mode GUI

The idea here is that we just redraw everything every frame. Then the scene graph is a simple function of the state.
