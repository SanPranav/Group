---
toc: false
comments: false
layout: post
title: Falling Obstacle Sprite
description: Ideas for the Cube Escape Sprite.
type: tangibles
courses: { compsci: {week: 1} }
---

  
<canvas id='canvas'></canvas>
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <style>
    body {
      margin: 0;
      overflow: hidden;
    }
    #canvas {
        margin: 0;
        border: 500px solid grey;
        width: 500px

    }

   
  </style>
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

   #fallingBlock () {
  position: absolute;
  width: 50px;
  height: 50px;
  background-color: blue;
  border-radius: 10px; /* Rounded corners for a softer look */
  box-shadow: 0 0 10px rgba(0, 0, 255, 0.7); /* Box shadow for a subtle glow effect */
  transition: background-color 0.3s ease; /* Smooth transition for background color changes */
}

</script>

</body>
</html