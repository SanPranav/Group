---
toc: false
comments: false
layout: post
title: Doodle Jump
description: A doodle jump Game Created by Xavier Celis. Use either the WAD or the Arrow keys to Move, try to get to the top and stay alive for the longest time to gain points.
type: hacks
courses: { compsci: {week: 2} }
---
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Doodle Jump</title>
    <style>
        canvas {
            border: 1px solid black;
            display: block;
            margin: 0 auto;
        }
        #scoreboard {
            position: absolute;
            top: 10px;
            right: 10px;
            font-size: 24px;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="600"></canvas>
    <div id="scoreboard">Score: 0</div>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        let doodler = {
            x: 200,
            y: 400,
            width: 20,
            height: 20,
            velocityY: 0,
            jumpPower: 10,
            speed: 20 // Speed for left and right movement
        };
        let platforms = [];
        let score = 0;
        let canvasMoveY = 0; // To track how much the canvas has moved upwards
        let isGameOver = false;
        function drawDoodler() {
            ctx.fillStyle = 'blue';
            ctx.fillRect(doodler.x, doodler.y, doodler.width, doodler.height);
        }
        function jump() {
            doodler.velocityY = -doodler.jumpPower;
        }
        function drawPlatforms() {
            ctx.fillStyle = 'green';
            platforms.forEach(platform => {
                ctx.fillRect(platform.x, platform.y - canvasMoveY, platform.width, platform.height);
            });
        }
        function updateScore() {
            ctx.fillStyle = 'white';
            ctx.font = '24px Arial';
            ctx.fillText('Score: ' + score, canvas.width - 150, 30);
        }
        function update() {
            // Clear the canvas
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            // Update doodler's position
            doodler.y += doodler.velocityY;
            doodler.velocityY += 0.5; // Gravity
            // Check for collisions with platforms
            platforms.forEach(platform => {
                if (doodler.x < platform.x + platform.width &&
                    doodler.x + doodler.width > platform.x &&
                    doodler.y + doodler.height > platform.y - canvasMoveY &&
                    doodler.y < platform.y + platform.height - canvasMoveY &&
                    doodler.velocityY > 0) {
                    // Collided with platform
                    doodler.y = platform.y - doodler.height + canvasMoveY;
                    doodler.velocityY = -doodler.jumpPower;
                    score += 100; // Increase score for landing on a platform
                }
            });
            // Update canvas movement
            if (doodler.y < canvas.height / 4) {
                platforms.forEach(platform => {
                    platform.y += -doodler.y;
                });
                doodler.y = canvas.height / 4;
                canvasMoveY += -doodler.y;
            }
            // Check if doodler is off screen
            if (doodler.y > canvas.height) {
                isGameOver = true;
            }
            // Draw doodler
            drawDoodler();
            // Draw platforms
            drawPlatforms();
            // Draw score
            updateScore();
            // Request animation frame
            if (!isGameOver) {
                requestAnimationFrame(update);
            } else {
                endGame();
            }
        }
        // Start the game
        update();
        // Event listener for jumping
        document.addEventListener('keydown', function(event) {
            if (event.code === 'KeyW') {
                jump();
            }
            // WASD movement
            if (event.code === 'KeyA' || event.code === 'ArrowLeft') {
                moveLeft();
            } else if (event.code === 'KeyD' || event.code === 'ArrowRight') {
                moveRight();
            }
        });
        function moveLeft() {
            doodler.x -= doodler.speed;
        }
        function moveRight() {
            doodler.x += doodler.speed;
        }
        function spawnPlatform() {
            platforms.push({
                x: Math.random() * (canvas.width - 50),
                y: platforms.length * 60 + canvasMoveY,
                width: 50,
                height: 10
            });
        }
        // Spawn initial platforms
        for (let i = 0; i < 10; i++) {
            spawnPlatform();
        }
        // Spawn new platforms as doodler goes up
        setInterval(function() {
            spawnPlatform();
        }, 2000); // Adjust this interval as needed
        function endGame() {
            ctx.fillStyle = 'red';
            ctx.font = '30px Arial';
            ctx.fillText('Game Over', canvas.width / 2 - 100, canvas.height / 2);
            // Listen for click to restart game
            canvas.addEventListener('click', restartGame);
        }
        function restartGame() {
            // Reset game state
            platforms = [];
            score = 0;
            canvasMoveY = 0;
            isGameOver = false;
            // Remove event listener
            canvas.removeEventListener('click', restartGame);
            // Spawn initial platforms
            for (let i = 0; i < 10; i++) {
                spawnPlatform();
            }
            // Restart game loop
            update();
        }
    function restartGame() {
    // Clear the canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    // Reset game state
    platforms = [];
    score = 0;
    canvasMoveY = 0;
    isGameOver = false;
    // Spawn initial platforms
    for (let i = 0; i < 10; i++) {
        spawnPlatform();
    }
    // Restart game loop
    update();
}
    </script>
</body>