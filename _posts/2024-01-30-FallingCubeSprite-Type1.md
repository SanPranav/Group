---
toc: false
comments: false
layout: post
title: Falling Obstacle Sprite
description: Ideas for the Cube Escape Sprite.
type: tangibles
courses: { compsci: {week: 1} }
---

<head>
    <style>
        canvas {
            border: 1px solid #000;
            display: block;
            margin: 20px auto;
        }
    </style>
</head>
<body>
    <canvas id="tetrisCanvas" width="300" height="600"></canvas>
    <script>
        const canvas = document.getElementById("tetrisCanvas");
        const context = canvas.getContext("2d");
        const ROWS = 20;
        const COLS = 10;
        const BLOCK_SIZE = 30;
        const board = Array.from({ length: ROWS }, () => Array(COLS).fill(0));
        function drawSquare(x, y, color) {
            context.fillStyle = color;
            context.fillRect(x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
            context.strokeStyle = "#000";
            context.strokeRect(x * BLOCK_SIZE, y * BLOCK_SIZE, BLOCK_SIZE, BLOCK_SIZE);
        }
        function drawBoard() {
            for (let row = 0; row < ROWS; row++) {
                for (let col = 0; col < COLS; col++) {
                    if (board[row][col]) {
                        drawSquare(col, row, board[row][col]);
                    }
                }
            }
        }
        function draw() {
            context.clearRect(0, 0, canvas.width, canvas.height);
            drawBoard();
        }
        // You can add more game logic and controls here
        // Example: 
        // setInterval(() => {
        //     draw();
        // }, 1000);
    </script>
</body>
</html>