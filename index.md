---
layout: base
title: Student Home
description: Home Page
hide: true
---



# Validate Notebook

<a href="https://github.com/Jack-Duffy/JackDuffy18/blob/main/_notebooks/Foundation/B-tools_and_equipment/2023-08-22-devops_tools-verify.ipynb" target="_blank" style="text-decoration: none;">
    <button style="
        padding: 10px 20px;
        background-color: #007BFF;
        color: white;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        font-size: 16px;">
        Notebook
    </button>
</a>



___________________________________________________________________________________________________________________________

# Fun Games!!!!!!

___________________________________________________________________________________________________________________________


<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Snake Game</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      margin: 0;
      padding: 0;
      background-color: #f4f4f4;
      overflow-y: auto;
    }

    .container {
      text-align: center;
      padding: 20px;
    }

    .screen {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 90%;
      max-width: 500px;
      padding: 20px;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
      border-radius: 10px;
      background-color: white;
      z-index: 10;
      display: none;
      flex-direction: column;
      justify-content: center;
      align-items: center;
    }

    .screen.active {
      display: flex;
    }

    .screen h1 {
      font-size: 36px;
      color: #2c3e50;
      margin-bottom: 20px;
    }

    .instructions {
      font-size: 18px;
      color: #555;
      text-align: center;
      margin-bottom: 20px;
    }

    .button {
      padding: 15px 30px;
      font-size: 20px;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    }

    .button:hover {
      background-color: #2980b9;
    }

    #gameSection {
      margin: 50px auto;
      max-width: 620px;
      padding: 20px;
      background-color: #e8f5e9;
      border: 2px solid #2c3e50;
      border-radius: 10px;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
    }

    canvas {
      display: block;
      margin: 20px auto;
      border: 5px solid #2c3e50;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
    }

    #gameContainer {
      text-align: center;
    }
  </style>
</head>
<body>
  <div id="gameContainer">
    <div class="screen" id="homeScreen">
      <h1>Welcome to Snake Game</h1>
      <p class="instructions">
        Use <b>WASD</b> or <b>Arrow Keys</b> to move the snake. Press <b>Space</b> to start the game. <br>
        Collect apples to grow your snake. Avoid hitting the edges or yourself!
      </p>
      <button class="button" id="startBtn">Start Game</button>
    </div>

    <div id="gameSection">
      <canvas id="gameCanvas" width="600" height="600" style="display: none;"></canvas>
    </div>

    <div class="screen" id="gameOverScreen">
      <h1>Game Over</h1>
      <button class="button" id="restartBtn">Restart</button>
      <button class="button" id="backHomeBtn1">Back to Home</button>
    </div>

    <div class="screen" id="victoryScreen">
      <h1>Congratulations, You Win!</h1>
      <button class="button" id="restartBtnVictory">Play Again</button>
      <button class="button" id="backHomeBtn2">Back to Home</button>
    </div>
  </div>

  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const startBtn = document.getElementById('startBtn');
    const restartBtn = document.getElementById('restartBtn');
    const restartBtnVictory = document.getElementById('restartBtnVictory');
    const backHomeBtn1 = document.getElementById('backHomeBtn1');
    const backHomeBtn2 = document.getElementById('backHomeBtn2');
    const homeScreen = document.getElementById('homeScreen');
    const gameOverScreen = document.getElementById('gameOverScreen');
    const victoryScreen = document.getElementById('victoryScreen');

    let gridSize = 20;
    let tileSize = canvas.width / gridSize;
    let snake = [];
    let food = {};
    let direction = 'RIGHT';
    let nextDirection = 'RIGHT';
    let gameInterval;
    let gameRunning = false;

    function initializeGame() {
      snake = [
        { x: 10, y: 10 },
        { x: 9, y: 10 },
        { x: 8, y: 10 }
      ];
      direction = 'RIGHT';
      nextDirection = 'RIGHT';
      generateFood();
      homeScreen.classList.remove('active');
      gameOverScreen.classList.remove('active');
      victoryScreen.classList.remove('active');
      canvas.style.display = 'block';
      gameRunning = true;
      startGameLoop();
    }

    function startGameLoop() {
      clearInterval(gameInterval);
      gameInterval = setInterval(() => {
        direction = nextDirection;
        moveSnake();
        if (checkCollision()) {
          endGame(false);
        } else if (snake.length >= gridSize * gridSize) {
          endGame(true);
        } else {
          renderGame();
        }
      }, 100);
    }

    function moveSnake() {
      const head = { ...snake[0] };
      switch (direction) {
        case 'UP': head.y--; break;
        case 'DOWN': head.y++; break;
        case 'LEFT': head.x--; break;
        case 'RIGHT': head.x++; break;
      }
      snake.unshift(head);
      if (head.x === food.x && head.y === food.y) {
        generateFood();
      } else {
        snake.pop();
      }
    }

    function checkCollision() {
      const head = snake[0];
      return (
        head.x < 0 || head.y < 0 ||
        head.x >= gridSize || head.y >= gridSize ||
        snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y)
      );
    }

    function generateFood() {
      food.x = Math.floor(Math.random() * gridSize);
      food.y = Math.floor(Math.random() * gridSize);
    }

    function renderGame() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      for (let y = 0; y < gridSize; y++) {
        for (let x = 0; x < gridSize; x++) {
          ctx.fillStyle = (x + y) % 2 === 0 ? '#8BC34A' : '#AED581';
          ctx.fillRect(x * tileSize, y * tileSize, tileSize, tileSize);
        }
      }
      snake.forEach((segment, index) => {
        const intensity = 255 - (index * (255 / snake.length));
        ctx.fillStyle = `rgb(0, 0, ${intensity})`;
        ctx.fillRect(segment.x * tileSize + 2, segment.y * tileSize + 2, tileSize - 4, tileSize - 4);
        if (index === 0) {
          // Draw eyes on the head
          ctx.fillStyle = 'white';
          ctx.beginPath();
          ctx.arc((segment.x + 0.3) * tileSize, (segment.y + 0.3) * tileSize, tileSize / 8, 0, 2 * Math.PI);
          ctx.arc((segment.x




___________________________________________________________________________________________________________________________