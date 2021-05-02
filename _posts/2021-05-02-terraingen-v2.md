---
title: Terrain Generator using Perlin Noise
author: Richard Hernandez
date: 2021-05-02 11:32:00 +0800 
categories: [Projects, Terrain Generator]
tags: [terrain, generator, p5, js, perlin, noise]
---

> **Toggle Rotate/Move**: this button will allow you to move using the arrow keys.
> **Toggle Boxes**: This will show the boxes that make up the terrain.

{% include terraingen.html %}

## Information
The code for this project can be found on the following links:

[See Code](https://editor.p5js.org/RHG101997/sketches/6IdZG40H0)  
[FullScreen](https://editor.p5js.org/RHG101997/full/6IdZG40H0)  

This project uses perlin noise to generate the height of the cubes and apply color. These then are categorized as mountains, grass, snow or water. There is also the addition of trees in green areas where the requirement are met. The parameters can be edited and played with.

```javascript

//Terrain size(Toggle Boxes will show this values)
let tileSize = 15; // each tile is an box
let mapSize = 20; // how many boxes 20x20 square

```



