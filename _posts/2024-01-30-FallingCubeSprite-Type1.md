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
    <title>Tetris</title>
    <style>
        canvas {
            border: 1px solid #000;
            display: block;
            margin: 20px auto;
        }
    </style>
</head>
<body>
    <canvas id="tetrisCanvas" width="650" height="675"></canvas>
    <script>
        const canvas = document.getElementById("tetrisCanvas");
        const context = canvas.getContext("2d");
        const ROWS = 20;
        const COLS = 21;
        const BLOCK_SIZE = 30;
        const board = Array.from({ length: ROWS }, () => Array(COLS).fill(0));
        const tetriminos = [
            [[1, 1, 1, 1]],  // I
            [[1, 1, 1], [1]], // L
            [[1, 1, 1], [0, 0, 1]], // J
            [[1, 1], [1, 1]], // O
            [[1, 1, 1], [0, 1]], // T
            [[1, 1], [1, 0, 0], [1]], // Z
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
        function draw() {
            context.clearRect(0, 0, canvas.width, canvas.height);
            drawBoard();
            drawTetrimino();
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
                // Game over, reset the board
                board.forEach(row => row.fill(0));
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
            if (event.code === "ArrowLeft") {
                moveLeft();
            } else if (event.code === "ArrowRight") {
                moveRight();
            } else if (event.code === "ArrowDown") {
                moveDown();
            } else if (event.code === "Space") {
                rotate();
            } 
        });
        function gameLoop() {
            moveDown();
            draw();
        }
        setInterval(gameLoop, 1000);
        spawnTetrimino();
        draw();
    </script>
</body>
</html>