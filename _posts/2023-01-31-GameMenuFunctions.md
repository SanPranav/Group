---
toc: True
comments: True
layout: post
title: Game Menu Buttons
description: Buttons on the Menu
courses: { compsci: {week: 3} }
type: hacks
---

<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Doodle Escape</title>
  <style>
    body {
      background: url("{{site.baseurl}}/images/sprite/TANKLOADINGBLANK.png") no-repeat center center fixed;
      color: #fff;
      font-family: 'Arial', sans-serif;
      text-align: center;
      position: relative;
      overflow: hidden;
      align: center;
    }
    body, html {
      max-width: 1472px;
      max-height: 828px;
    }
    .button {
      background-color: transparent;
      color: #fff;
      font-size: 16px;
      cursor: pointer;
      font-family: 'Arial', sans-serif;
      border: none;
      outline: none;
      position: fixed;
      transform: translateX(-50%);
      transition: box-shadow 0.3s ease-out, color 0.3s ease-out;
      z-index: 1;
    }
    .buttons #instructionsButton {
      top: 53vh;
      left: 37vw;
    }
    .buttons #settingsButton {
      top: 57vh;
      left: 13vw;
      transform: translate(-50%, -50%);
    }
    .buttons #playButton {
      top: 55vh;
      left: 25vw;
      transform: translate(-50%, -50%);
    }
    .button:hover {
      box-shadow: 0 0 20px #ff4500, 0 0 40px #ff4500;
      color: #ff4500;
      animation: shake 0.5s ease-in-out infinite;
    }
    @keyframes shake {
      0%, 100% {
        transform: translate(-50%, -50%);
      }
      25%, 75% {
        transform: translate(-55%, -50%);
      }
      50% {
        transform: translate(-50%, -50%);
      }
    }
    .overlay {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(0, 0, 0, 0.5);
      z-index: 2;
    }
    .model {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 0 20px rgba(0, 0, 0, 0.5);
      z-index: 3;
    }
    #gameContainer {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      }
  </style>
</head>
<body>
  <div class="buttons">
    <button id="settingsButton" class="button" onclick="openSettings()">Settings</button>
    <button id="playButton" class="button" onclick="startGame()">Play</button>
    <button id="instructionsButton" class="button" onclick="openInstructions()">Instructions</button>
  </div>

  <div id="settingsOverlay" class="overlay">
    <div class="model">
      <!-- add stuff -->
      <h2>Settings</h2>
      <!-- add stuff -->
      <button onclick="closeSettings()">Close</button>
    </div>
  </div>

  <div id="instructionsOverlay" class="overlay">
    <div class="model">
      <!-- add stuff -->
      <h2>Instructions</h2>
      <!-- add stuff -->
      <button onclick="closeInstructions()">Close</button>
    </div>
  </div>
