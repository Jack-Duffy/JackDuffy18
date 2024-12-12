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
    }

    .container {
      text-align: center;
      padding: 20px;
    }

    .home-screen, .game-over, .victory {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-color: rgba(255, 255, 255, 0.95);
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      z-index: 10;
    }

    .home-screen h1, .game-over h1, .victory h1 {
      font-size: 48px;
      color: #2c3e50;
      margin-bottom: 20px;
    }

    .instructions {
      font-size: 18px;
      color: #555;
      max-width: 600px;
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

    canvas {
      display: block;
      margin: 20px auto;
      border: 5px solid #2c3e50;
      box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
    }
  </style>
</head>
<body>
  <div class="home-screen" id="homeScreen">
    <h1>Welcome to Snake Game</h1>
    <p class="instructions">
      Use <b>WASD</b> or <b>Arrow Keys</b> to move the snake. Press <b>Space</b> to start the game. <br>
      Collect apples to grow your snake. Avoid hitting the edges or yourself!
    </p>
    <button class="button" id="startBtn">Start Game</button>
  </div>

  <canvas id="gameCanvas" width="600" height="600"></canvas>

  <div class="game-over" id="gameOverScreen" style="display: none;">
    <h1>Game Over</h1>
    <button class="button" id="restartBtn">Restart</button>
  </div>

  <div class="victory" id="victoryScreen" style="display: none;">
    <h1>Congratulations, You Win!</h1>
    <button class="button" id="restartBtnVictory">Play Again</button>
  </div>

  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const startBtn = document.getElementById('startBtn');
    const restartBtn = document.getElementById('restartBtn');
    const restartBtnVictory = document.getElementById('restartBtnVictory');
    const homeScreen = document.getElementById('homeScreen');
    const gameOverScreen = document.getElementById('gameOverScreen');
    const victoryScreen = document.getElementById('victoryScreen');

    let gridSize = 20;
    let tileSize = canvas.width / gridSize;
    let snake = [];
    let food = {};
    let direction = 'RIGHT';
    let gameInterval;
    let gameRunning = false;

    function initializeGame() {
      snake = [
        { x: 10, y: 10 },
        { x: 9, y: 10 },
        { x: 8, y: 10 }
      ];
      direction = 'RIGHT';
      generateFood();
      homeScreen.style.display = 'none';
      gameOverScreen.style.display = 'none';
      victoryScreen.style.display = 'none';
      gameRunning = true;
      startGameLoop();
    }

    function startGameLoop() {
      clearInterval(gameInterval);
      gameInterval = setInterval(() => {
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
      renderBackground();
      renderSnake();
      renderFood();
    }

    function renderBackground() {
      for (let y = 0; y < gridSize; y++) {
        for (let x = 0; x < gridSize; x++) {
          ctx.fillStyle = (x + y) % 2 === 0 ? '#8BC34A' : '#AED581';
          ctx.fillRect(x * tileSize, y * tileSize, tileSize, tileSize);
        }
      }
    }

    function renderSnake() {
      snake.forEach((segment, index) => {
        const intensity = 255 - (index * (255 / snake.length));
        ctx.fillStyle = `rgb(0, 0, ${intensity})`;
        ctx.fillRect(segment.x * tileSize, segment.y * tileSize, tileSize, tileSize);
      });
    }

    function renderFood() {
      ctx.fillStyle = '#D84315';
      ctx.fillRect(food.x * tileSize + 2, food.y * tileSize + 2, tileSize - 4, tileSize - 4);
      ctx.fillStyle = '#6D4C41';
      ctx.fillRect(food.x * tileSize + tileSize / 2 - 2, food.y * tileSize - 4, 4, 8);
    }

    function endGame(isVictory) {
      gameRunning = false;
      clearInterval(gameInterval);
      if (isVictory) {
        victoryScreen.style.display = 'flex';
      } else {
        gameOverScreen.style.display = 'flex';
      }
    }

    function changeDirection(event) {
      if (!gameRunning) return;
      const key = event.key.toUpperCase();
      if (key === 'W' && direction !== 'DOWN') direction = 'UP';
      if (key === 'S' && direction !== 'UP') direction = 'DOWN';
      if (key === 'A' && direction !== 'RIGHT') direction = 'LEFT';
      if (key === 'D' && direction !== 'LEFT') direction = 'RIGHT';
    }

    startBtn.addEventListener('click', initializeGame);
    restartBtn.addEventListener('click', initializeGame);
    restartBtnVictory.addEventListener('click', initializeGame);
    document.addEventListener('keydown', changeDirection);
    window.addEventListener('keydown', (event) => {
      if (event.key === ' ') initializeGame();
    });
  </script>
</body>
</html>



___________________________________________________________________________________________________________________________