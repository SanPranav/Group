---
comments: True
layout: post
title: Aim Trainer
description: Aim Training Clicking Game
type: hacks
courses: {'compsci': {'week': 0}}
categories: ['C4.1']
---
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Aim Trainer</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
        }

        canvas {
            border: 2px solid #000000;
            background-color: #ffffff;
            width: 800px;
            height: 800px;
            margin-bottom: 20px;
        }

        .container {
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .options {
            background: linear-gradient(to bottom, #b3e0ff, #e6f3ff);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        .options h2 {
            margin-bottom: 10px;
        }

        .option {
            margin-bottom: 10px;
        }

        button {
            padding: 10px 20px;
            border: none;
            border-radius: 5px;
            background-color: #4CAF50;
            color: white;
            cursor: pointer;
        }

        button:hover {
            background-color: #45a049;
        }
    </style>
</head>
<body>

<div class="container">
    <canvas id="gameCanvas"></canvas>
    <div class="options">
        <h2>Select Number of Lives:</h2>
        <div class="option">
            <button onclick="setLives(10)">10 Lives</button>
        </div>
        <div class="option">
            <button onclick="setLives(25)">25 Lives</button>
        </div>
        <div class="option">
            <button onclick="setLives(50)">50 Lives</button>
        </div>
    </div>
</div>

<script>
// Initialize canvas
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
canvas.width = 800;
canvas.height = 800;

// Colors
const WHITE = '#FFFFFF';
const RED = '#FF0000';
const BLACK = '#000000';

// Dot parameters
const DOT_RADIUS = 30;
const MAX_SPEED = 5;
const MIN_SPEED = 1;
const MAX_DOTS = 5;

// Game parameters
let lives = 25;

// Initialize variables
let score = 0;
let highScore = localStorage.getItem('highScore') || 0;
let dots = [];
let gameStarted = false;

// Function to create a new dot
function newDot() {
    const x = Math.floor(Math.random() * (canvas.width - DOT_RADIUS * 2) + DOT_RADIUS);
    const y = Math.floor(Math.random() * (canvas.height - DOT_RADIUS * 2) + DOT_RADIUS);
    const speed = Math.random() * (MAX_SPEED - MIN_SPEED) + MIN_SPEED;
    return { x, y, speed };
}

// Function to draw the dots
function drawDot(dot) {
    ctx.beginPath();
    ctx.arc(dot.x, dot.y, DOT_RADIUS, 0, Math.PI * 2);
    ctx.fillStyle = RED;
    ctx.fill();
    ctx.closePath();
}

// Function to update the position of the dots
function updateDot(dot) {
    dot.x += dot.speed;
    return dot.x > canvas.width + DOT_RADIUS;
}

// Function to display score and lives
function displayText() {
    ctx.fillStyle = BLACK;
    ctx.font = '24px Arial';
    ctx.fillText(`Score: ${score}`, 10, 30);
    ctx.fillText(`High Score: ${highScore}`, 10, 60);
    ctx.fillText(`Lives: ${lives}`, 10, 90);
}

// Function to display start screen
function displayStartScreen() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = BLACK;
    ctx.font = '36px Arial';
    ctx.fillText('Aim Trainer', canvas.width / 2 - 100, canvas.height / 2 - 50);
    ctx.fillText('Click to Start', canvas.width / 2 - 120, canvas.height / 2);
}

// Function to display restart screen
function displayRestartScreen() {
    ctx.clearRect(0, 0, canvas.width, canvas.height);
    ctx.fillStyle = BLACK;
    ctx.font = '36px Arial';
    ctx.fillText('Game Over', canvas.width / 2 - 100, canvas.height / 2 - 50);
    ctx.fillText(`Final Score: ${score}`, canvas.width / 2 - 120, canvas.height / 2);
    ctx.fillText('Click to Restart', canvas.width / 2 - 140, canvas.height / 2 + 50);
}

// Main game loop
function gameLoop() {
    if (!gameStarted) {
        displayStartScreen();
        return;
    }

    if (lives <= 0) {
        gameOver();
        return;
    }

    ctx.clearRect(0, 0, canvas.width, canvas.height);
    displayText();

    // Generate new dots
    if (dots.length < MAX_DOTS) {
        dots.push(newDot());
    }

    // Update and draw dots
    for (let i = dots.length - 1; i >= 0; i--) {
        const dot = dots[i];
        if (updateDot(dot)) {
            lives--;
            dots.splice(i, 1);
        } else {
            drawDot(dot);
        }
    }

    requestAnimationFrame(gameLoop);
}

// Game over
function gameOver() {
    displayRestartScreen();
    if (score > highScore) {
        highScore = score;
        localStorage.setItem('highScore', highScore);
    }
}

// Event listener for mouse click
canvas.addEventListener('click', (event) => {
    if (!gameStarted) {
        gameStarted = true;
        score = 0;
        lives = 25; // Reset lives
        dots = [];
        gameLoop();
    } else if (lives > 0) {
        const mouseX = event.clientX - canvas.getBoundingClientRect().left;
        const mouseY = event.clientY - canvas.getBoundingClientRect().top;

        for (let i = dots.length - 1; i >= 0; i--) {
            const dot = dots[i];
            const distance = Math.sqrt((mouseX - dot.x) ** 2 + (mouseY - dot.y) ** 2);
            if (distance <= DOT_RADIUS) {
                score += Math.round(MAX_SPEED - dot.speed + 1);
                dots.splice(i, 1);
                break;
            }
        }
    }
});

// Function to set the number of lives
function setLives(numLives) {
    lives = numLives;
}

// Start the game loop
gameLoop();
</script>

</body>
</html>

</body>
</html>
