---
toc: false
comments: false
layout: post
title: Tic Tac Toe
description: A game made by Pranav Santhosh. Click On Boxes And Get 3 in a row to win! Reload the page to restart.
type: plans
courses: { compsci: {week: 1} }
---

<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Tic Tac Toe</title>
  <style>
    body {
      font-family: Arial, sans-serif;
    }

    #game-board {
      display: grid;
      grid-template-columns: repeat(3, 100px);
      grid-gap: 10px;
      margin-top: 20px;
    }

    .cell {
      width: 100px;
      height: 100px;
      border: 2px solid #333;
      display: flex;
      align-items: center;
      justify-content: center;
      font-size: 24px;
      cursor: pointer;
    }

    .cell:hover {
      background-color: #f0f0f0;
    }

    #message {
      margin-top: 20px;
      font-size: 20px;
    }
  </style>
</head>
<body>

<div id="game-board"></div>

<div id="message"></div>

<script>
  let currentPlayer;
  let board;
  let gameOver;

  function startGame() {
    currentPlayer = 'X';
    board = ['', '', '', '', '', '', '', '', ''];
    gameOver = false;

    document.getElementById('game-board').innerHTML = '';
    document.getElementById('message').innerText = '';

    for (let i = 0; i < 9; i++) {
      const cell = document.createElement('div');
      cell.className = 'cell';
      cell.dataset.index = i;
      cell.addEventListener('click', handleCellClick);
      document.getElementById('game-board').appendChild(cell);
    }
  }

  function handleCellClick(event) {
    if (gameOver) return;

    const index = event.target.dataset.index;
    if (board[index] === '') {
      board[index] = currentPlayer;
      renderBoard();

      if (checkWin()) {
        gameOver = true;
        document.getElementById('message').innerText = `Player ${currentPlayer} wins!`;
      } else if (board.every(cell => cell !== '')) {
        gameOver = true;
        document.getElementById('message').innerText = "It's a draw!";
      } else {
        currentPlayer = currentPlayer === 'X' ? 'O' : 'X';
      }
    }
  }

  function renderBoard() {
    const cells = document.querySelectorAll('.cell');
    cells.forEach((cell, index) => {
      cell.innerText = board[index];
    });
  }

  function checkWin() {
    const winPatterns = [
      [0, 1, 2], [3, 4, 5], [6, 7, 8], // Rows
      [0, 3, 6], [1, 4, 7], [2, 5, 8], // Columns
      [0, 4, 8], [2, 4, 6]             // Diagonals
    ];

    for (const pattern of winPatterns) {
      const [a, b, c] = pattern;
      if (board[a] !== '' && board[a] === board[b] && board[a] === board[c]) {
        return true;
      }
    }

    return false;
  }

  startGame();
</script>

</body>
</html>
