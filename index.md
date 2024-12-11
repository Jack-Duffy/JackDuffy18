---
layout: base
title: Student Home
description: Home Page
hide: true
---
-

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Snake Game</title>
  <style>
    body {
      font-family: 'Arial', sans-serif;
      background-color: #f4f4f4;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      overflow: hidden;
    }

    canvas {
      border: 3px solid black;
      background-color: #e6f7ff;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.5);
    }

    .button-container {
      position: absolute;
      top: 20px;
      display: flex;
      gap: 10px;
    }

    .button {
      padding: 10px 20px;
      background-color: #3498db;
      color: white;
      border: none;
      border-radius: 5px;
      cursor: pointer;
      font-size: 16px;
    }

    .button:hover {
      background-color: #2980b9;
    }

    .game-over, .victory {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      text-align: center;
      font-size: 36px;
      color: #2c3e50;
      display: none;
    }

    .game-over h1, .victory h1 {
      margin: 0;
      font-size: 48px;
      color: #e74c3c;
    }
  </style>
</head>
<body>
  <canvas id="gameCanvas" width="600" height="600"></canvas>
  <div class="button-container">
    <button class="button" id="startBtn">Start Game</button>
    <button class="button" id="restartBtn">Restart Game</button>
  </div>
  <div class="game-over">
    <h1>Game Over!</h1>
    <p>Press 'R' to Restart</p>
  </div>
  <div class="victory">
    <h1>You Win!</h1>
    <p>Press 'R' to Restart</p>
  </div>

  <script>
    const canvas = document.getElementById('gameCanvas');
    const ctx = canvas.getContext('2d');
    const startBtn = document.getElementById('startBtn');
    const restartBtn = document.getElementById('restartBtn');
    const gameOverScreen = document.querySelector('.game-over');
    const victoryScreen = document.querySelector('.victory');
    const gridSize = 20;
    const tileSize = canvas.width / gridSize;

    let snake, food, direction, gameInterval;

    function initializeGame() {
      snake = [
        { x: 10, y: 10 },
        { x: 9, y: 10 },
        { x: 8, y: 10 }
      ];
      food = { x: Math.floor(Math.random() * gridSize), y: Math.floor(Math.random() * gridSize) };
      direction = 'RIGHT';
      gameOverScreen.style.display = 'none';
      victoryScreen.style.display = 'none';
      renderGame();
      startGameLoop();
    }

    function startGameLoop() {
      gameInterval = setInterval(() => {
        moveSnake();
        if (checkCollision()) {
          clearInterval(gameInterval);
          gameOverScreen.style.display = 'block';
        }
        if (checkVictory()) {
          clearInterval(gameInterval);
          victoryScreen.style.display = 'block';
        }
        renderGame();
      }, 100);
    }

    function moveSnake() {
      const head = { ...snake[0] };

      switch (direction) {
        case 'UP':
          head.y--;
          break;
        case 'DOWN':
          head.y++;
          break;
        case 'LEFT':
          head.x--;
          break;
        case 'RIGHT':
          head.x++;
          break;
      }

      snake.unshift(head);
      if (head.x === food.x && head.y === food.y) {
        food = { x: Math.floor(Math.random() * gridSize), y: Math.floor(Math.random() * gridSize) };
      } else {
        snake.pop();
      }
    }

    function checkCollision() {
      const head = snake[0];
      return (
        head.x < 0 ||
        head.y < 0 ||
        head.x >= gridSize ||
        head.y >= gridSize ||
        snake.slice(1).some(part => part.x === head.x && part.y === head.y)
      );
    }

    function checkVictory() {
      return snake.length === gridSize * gridSize;
    }

    function renderGame() {
      ctx.clearRect(0, 0, canvas.width, canvas.height);

      snake.forEach((part, index) => {
        if (index === 0) {
          ctx.fillStyle = 'green';
          ctx.fillRect(part.x * tileSize, part.y * tileSize, tileSize, tileSize);
          // Snake eyes
          ctx.fillStyle = 'black';
          ctx.beginPath();
          ctx.arc(part.x * tileSize + tileSize / 4, part.y * tileSize + tileSize / 4, 3, 0, Math.PI * 2);
          ctx.arc(part.x * tileSize + 3 * tileSize / 4, part.y * tileSize + tileSize / 4, 3, 0, Math.PI * 2);
          ctx.fill();
        } else {
          ctx.fillStyle = 'darkgreen';
          ctx.fillRect(part.x * tileSize, part.y * tileSize, tileSize, tileSize);
        }
      });

      ctx.fillStyle = 'red';
      ctx.fillRect(food.x * tileSize, food.y * tileSize, tileSize, tileSize);
    }

    function changeDirection(event) {
      switch (event.key) {
        case 'w':
        case 'ArrowUp':
          if (direction !== 'DOWN') direction = 'UP';
          break;
        case 's':
        case 'ArrowDown':
          if (direction !== 'UP') direction = 'DOWN';
          break;
        case 'a':
        case 'ArrowLeft':
          if (direction !== 'RIGHT') direction = 'LEFT';
          break;
        case 'd':
        case 'ArrowRight':
          if (direction !== 'LEFT') direction = 'RIGHT';
          break;
      }
    }

    function restartGame() {
      initializeGame();
    }

    startBtn.addEventListener('click', initializeGame);
    restartBtn.addEventListener('click', restartGame);
    document.addEventListener('keydown', changeDirection);

    window.addEventListener('keydown', (event) => {
      if (event.key === 'r' || event.key === 'R') {
        restartGame();
      }
    });

  </script>
</body>
</html>

