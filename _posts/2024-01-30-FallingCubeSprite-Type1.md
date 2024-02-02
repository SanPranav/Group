---
toc: false
comments: false
layout: post
title: Falling Obstacle Sprite
description: Ideas for the Cube Escape Sprite.
type: tangibles
courses: { compsci: {week: 1} }
---

  
<style>
    #canvas {
        margin: 0;
        border: 10px solid grey;
    }
</style>
<canvas id='canvas'></canvas>
<script>
    // Create empty canvas
    let canvas = document.getElementById('canvas');
    let c = canvas.getContext('2d');
    // Set the canvas dimensions
    canvas.width = 650;
    canvas.height = 660;
</script>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Falling Block Animation with Randomized Reset</title>
</head>
<body>

<div id="fallingBlock"></div>

<script>
  const fallingBlock = document.getElementById('fallingBlock');
  const fallSpeed = 3; // Adjust the speed as needed
  const windowHeight = window.innerHeight;

  function animate() {
    const positionY = parseFloat(fallingBlock.style.top) || 0;
    const newPositionY = positionY + fallSpeed;
    fallingBlock.style.top = `${newPositionY}px`;

    // Reset the position with a random value when the block reaches the bottom
    if (newPositionY >= windowHeight - 50) {
      fallingBlock.style.top = '0';
      fallingBlock.style.left = `${Math.random() * (window.innerWidth - 50)}px`;
    }
  }

  // Using setInterval for the loop
  const animationInterval = setInterval(animate, 16); // 60 FPS

  // Uncomment the next line if you prefer requestAnimationFrame
  // function animateLoop() { animate(); requestAnimationFrame(animateLoop); }
  // animateLoop();

 class Block  {
        constructor() {
            // Initial position and velocity of the player
            this.position = {
                x: 100,
                y: 200
            };
            this.velocity = {
                x: 0,
                y: 0
            };
            // Dimensions of the player
            this.width = 30;
            this.height = 30;
        }
        // Method to draw the player on the canvas
        draw() {
            c.fillStyle = 'blue';
            c.fillRect(this.position.x, this.position.y, this.width, this.height);
        }
 }
</script>

</body>
</html