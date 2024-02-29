---
toc: false
comments: false
layout: post
title: Tetrix
description: A Tetris Game Created by Xavier Celis. Use W, A, and D to move the block and S to flip it. Score the most points to win!
type: hacks
courses: { compsci: {week: 1} }
---

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tetris</title>
    <style>
        canvas {
            border: 1px solid #000;
            display: grid;
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
        let speed = 500; // milliseconds
        let score = 0;
        let gameOver = false;
        const board = Array.from({ length: ROWS }, () => Array(COLS).fill(0));
        const tetriminos = [
            [[1, 1, 1, 1]],  // I
            [[1, 1, 1], [1]], // L
            [[1, 1, 1], [0, 0, 1]], // J
            [[1, 1], [1, 1]], // O
            [[1, 1, 1], [0, 1]], // T
            [[1, 1], [0, 1, 1]], // S
        ];
        let currentTetrimino;
        let currentX = 0;
        let currentY = 0;
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
        function drawTetrimino() {
            for (let row = 0; row < currentTetrimino.length; row++) {
                for (let col = 0; col < currentTetrimino[row].length; col++) {
                    if (currentTetrimino[row][col]) {
                        drawSquare(currentX + col, currentY + row, "blue");
                    }
                }
            }
        }
        function drawScore() {
            context.fillStyle = "white";
            context.font = "20px Arial";
            context.fillText("Score: " + score, 10, 30);
        }
        function drawGameOver() {
            context.fillStyle = "red";
            context.font = "30px Arial";
            context.fillText("Game Over!", 70, canvas.height / 2);
            // Add text for retry instructions
            context.font = "20px Arial";
            context.fillText("Click anywhere to retry", 50, canvas.height / 2 + 50);
        }
        function draw() {
            context.clearRect(0, 0, canvas.width, canvas.height);
            drawBoard();
            drawTetrimino();
            drawScore();
            if (gameOver) {
                drawGameOver();
                // Add event listener to retry on click
                canvas.addEventListener("click", retryGame);
            } else {
                // Remove event listener if game is ongoing
                canvas.removeEventListener("click", retryGame);
            }
        }
        function collide() {
            for (let row = 0; row < currentTetrimino.length; row++) {
                for (let col = 0; col < currentTetrimino[row].length; col++) {
                    if (currentTetrimino[row][col] && (board[currentY + row] && board[currentY + row][currentX + col]) !== 0) {
                        return true;
                    }
                }
            }
            return false;
        }
        function merge() {
            for (let row = 0; row < currentTetrimino.length; row++) {
                for (let col = 0; col < currentTetrimino[row].length; col++) {
                    if (currentTetrimino[row][col]) {
                        board[currentY + row][currentX + col] = "blue";
                    }
                }
            }
            checkRows();
            checkGameOver();
        }
        function checkRows() {
            for (let row = ROWS - 1; row >= 0; row--) {
                if (board[row].every(cell => cell !== 0)) {
                    board.splice(row, 1);
                    board.unshift(Array(COLS).fill(0));
                    score += 100; // Increase score when a row is deleted
                }
            }
        }
        function rotateTetrimino() {
            const newTetrimino = [];
            for (let col = 0; col < currentTetrimino[0].length; col++) {
                newTetrimino[col] = [];
                for (let row = currentTetrimino.length - 1; row >= 0; row--) {
                    newTetrimino[col][currentTetrimino.length - 1 - row] = currentTetrimino[row][col];
                }
            }
            return newTetrimino;
        }
        function moveLeft() {
            currentX -= 1;
            if (collide()) {
                currentX += 1;
            }
        }
        function moveRight() {
            currentX += 1;
            if (collide()) {
                currentX -= 1;
            }
        }
        function moveDown() {
            currentY += 1;
            if (collide()) {
                currentY -= 1;
                merge();
                spawnTetrimino();
            }
        }
        function spawnTetrimino() {
            currentTetrimino = tetriminos[Math.floor(Math.random() * tetriminos.length)];
            currentX = Math.floor((COLS - currentTetrimino[0].length) / 2);
            currentY = 0;
            if (collide()) {
                gameOver = true;
            }
        }
        function rotate() {
            const rotatedTetrimino = rotateTetrimino();
            if (!checkCollision(rotatedTetrimino)) {
                currentTetrimino = rotatedTetrimino;
            }
        }
        function checkCollision(newTetrimino) {
            for (let row = 0; row < newTetrimino.length; row++) {
                for (let col = 0; col < newTetrimino[row].length; col++) {
                    if (newTetrimino[row][col] && (board[currentY + row] && board[currentY + row][currentX + col]) !== 0) {
                        return true;
                    }
                }
            }
            return false;
        }
        document.addEventListener("keydown", function (event) {
            if (!gameOver) {
                if (event.code === "KeyW" || event.code === "ArrowUp") {
                    rotate();
                } else if (event.code === "KeyA" || event.code === "ArrowLeft") {
                    moveLeft();
                } else if (event.code === "KeyD" || event.code === "ArrowRight") {
                    moveRight();
                } else if (event.code === "KeyS" || event.code === "ArrowDown") {
                    moveDown();
                }
            }
        });
        function checkGameOver() {
            if (board[0].some(cell => cell !== 0)) {
                gameOver = true;
            }
        }
        function gameLoop() {
            if (!gameOver) {
                moveDown();
            }
            draw();
            if (!gameOver) {
                setTimeout(gameLoop, speed);
            }
        }
        function retryGame() {
            score = 0;
            gameOver = false;
            board.forEach(row => row.fill(0));
            draw();
            gameLoop();
        }
        spawnTetrimino();
        draw();
        gameLoop();
    </script>
</body>