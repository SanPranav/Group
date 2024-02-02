---
toc: false
comments: false
layout: post
title: Falling Obstacle Sprite
description: Ideas for the Cube Escape Sprite.
type: tangibles
courses: { compsci: {week: 1} }
---

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

    #fallingBlock {
      position: absolute;
      width: 50px;
      height: 50px;
      background-color: blue;
    }
  </style>
  <title>Falling Block Animation</title>
</head>
<body>

<div id="fallingBlock"></div>

<script>
  const fallingBlock = document.getElementById('fallingBlock');
  let positionY = 0;
  const fallSpeed = 5; // Adjust the speed as needed

  function animate() {
    positionY += fallSpeed;
    fallingBlock.style.top = `${positionY}px`;

    // Stop the animation when the block reaches the bottom (optional)
    const windowHeight = window.innerHeight;
    if (positionY >= windowHeight - 50) {
      clearInterval(animationInterval);
    }
  }

  // Choose either setInterval or requestAnimationFrame
  // For simplicity, using setInterval in this example
  const animationInterval = setInterval(animate, 16); // 60 FPS

  // Uncomment the next line if you prefer requestAnimationFrame
  // function animateLoop() { animate(); requestAnimationFrame(animateLoop); }
  // animateLoop();
</script>

</body>
</html>
