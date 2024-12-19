---
layout: base
title: Snake
permalink: /snake/
---

# Snake Game

This is a complete Snake game implementation. Copy the following code into `snake.md` and view it via a Git.io link or in a Markdown renderer that supports inline HTML, CSS, and JavaScript.

# Snake Game Code

Copy the following code blocks into a single `snake.md` file.

---

## Part 1: HTML Structure
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Snake Game</title>
  <style>
    /* General styling */
    body {
      margin: 0;
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      background-color: #f0f0f0;
    }

    canvas {
      border: 10px solid darkgreen;
      background: repeating-linear-gradient(
          0deg,
          lightgreen 0px,
          lightgreen 20px,
          green 20px,
          green 40px
        ),
        repeating-linear-gradient(
          90deg,
          lightgreen 0px,
          lightgreen 20px,
          green 20px,
          green 40px
        );
    }

    button {
      margin: 10px;
      padding: 10px 20px;
      font-size: 16px;
    }

    #menu, #setting, #gameover {
      text-align: center;
    }
  </style>
</head>
<body>
  <!-- Main Menu -->
  <div id="menu">
    <button id="new_game">New Game</button>
    <button id="setting_menu">Settings</button>
  </div>

  <!-- Settings Menu -->
  <div id="setting" style="display: none;">
    <h3>Settings</h3>
    <label>Wall Collisions: 
      <input type="radio" name="wall" value="true" checked> On
      <input type="radio" name="wall" value="false"> Off
    </label>
    <br>
    <label>Speed: 
      <input type="radio" name="speed" value="200" checked> Easy
      <input type="radio" name="speed" value="100"> Medium
      <input type="radio" name="speed" value="50"> Hard
    </label>
    <br>
    <button id="new_game1">Start Game</button>
  </div>

  <!-- Game Over Screen -->
  <div id="gameover" style="display: none;">
    <h3>Game Over</h3>
    <p>Score: <span id="score_value">0</span></p>
    <button id="new_game2">Play Again</button>
  </div>

  <canvas id="snake" width="400" height="400"></canvas>
</body>

<script>
/* Attributes of Game */
/////////////////////////////////////////////////////////////
// Canvas & Context
const canvas = document.getElementById("snake");
const ctx = canvas.getContext("2d");

// HTML Game IDs
const SCREEN_SNAKE = 0;
const SCREEN_MENU = -1, SCREEN_GAME_OVER = 1, SCREEN_SETTING = 2;
const screen_menu = document.getElementById("menu");
const screen_game_over = document.getElementById("gameover");
const screen_setting = document.getElementById("setting");
const ele_score = document.getElementById("score_value");

// HTML Event IDs
const button_new_game = document.getElementById("new_game");
const button_new_game1 = document.getElementById("new_game1");
const button_new_game2 = document.getElementById("new_game2");
const button_setting_menu = document.getElementById("setting_menu");

// Game Control
const BLOCK_SIZE = 20; // size of each block
let snake = [];
let snake_dir = { x: 0, y: 0 };
let food = { x: 0, y: 0 };
let score = 0;
let gameInterval;
let wall = true; // Enable wall collisions by default
let speed = 200; // Default speed

/* Initialize game */
function initializeGame() {
  snake = [{ x: 200, y: 200 }];
  snake_dir = { x: 0, y: 0 };
  score = 0;
  generateFood();
  showScreen(SCREEN_SNAKE);
  clearInterval(gameInterval);
  gameInterval = setInterval(gameLoop, speed);
}

function generateFood() {
  food.x = Math.floor(Math.random() * (canvas.width / BLOCK_SIZE)) * BLOCK_SIZE;
  food.y = Math.floor(Math.random() * (canvas.height / BLOCK_SIZE)) * BLOCK_SIZE;
  food.color = ["red", "orange", "yellow"][Math.floor(Math.random() * 3)];
}
/* Game Loop */
function gameLoop() {
  if (!snake_dir.x && !snake_dir.y) return;

  // Clear screen
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Draw elements
  drawFood();
  drawSnake();
  moveSnake();

  // Check collisions
  const head = snake[0];
  if (wall && (head.x < 0 || head.y < 0 || head.x >= canvas.width || head.y >= canvas.height)) {
    gameOver();
  } else if (!wall) {
    head.x = (head.x + canvas.width) % canvas.width;
    head.y = (head.y + canvas.height) % canvas.height;
  }

  if (snake.some((segment, index) => index !== 0 && segment.x === head.x && segment.y === head.y)) {
    gameOver();
  }
}

function drawSnake() {
  snake.forEach((segment, index) => {
    ctx.fillStyle = "blue";
    ctx.fillRect(segment.x, segment.y, BLOCK_SIZE, BLOCK_SIZE);

    // Add eyes to the snake's head
    if (index === 0) {
      ctx.fillStyle = "white";
      ctx.fillRect(segment.x + 5, segment.y + 5, 5, 5);
      ctx.fillRect(segment.x + 10, segment.y + 5, 5, 5);
    }
  });
}

function drawFood() {
  ctx.fillStyle = food.color;
  ctx.fillRect(food.x, food.y, BLOCK_SIZE, BLOCK_SIZE);
}

function moveSnake() {
  const head = { x: snake[0].x + snake_dir.x, y: snake[0].y + snake_dir.y };
  snake.unshift(head);

  // Check if food is eaten
  if (head.x === food.x && head.y === food.y) {
    score++;
    generateFood();
  } else {
    snake.pop();
  }
}

function gameOver() {
  clearInterval(gameInterval);
  showScreen(SCREEN_GAME_OVER);
  ele_score.textContent = score;
}

/* Input Handling */
document.addEventListener("keydown", (e) => {
  if (e.key === "ArrowUp" || e.key === "w") snake_dir = { x: 0, y: -BLOCK_SIZE };
  if (e.key === "ArrowDown" || e.key === "s") snake_dir = { x: 0, y: BLOCK_SIZE };
  if (e.key === "ArrowLeft" || e.key === "a") snake_dir = { x: -BLOCK_SIZE, y: 0 };
  if (e.key === "ArrowRight" || e.key === "d") snake_dir = { x: BLOCK_SIZE, y: 0 };
});

/* Display Management */
function showScreen(screen) {
  screen_menu.style.display = screen === SCREEN_MENU ? "block" : "none";
  screen_game_over.style.display = screen === SCREEN_GAME_OVER ? "block" : "none";
  screen_setting.style.display = screen === SCREEN_SETTING ? "block" : "none";
  canvas.style.display = screen === SCREEN_SNAKE ? "block" : "none";
}

/* Event Listeners */
button_new_game.onclick = initializeGame;
button_new_game1.onclick = initializeGame;
button_new_game2.onclick = initializeGame;
button_setting_menu.onclick = () => showScreen(SCREEN_SETTING);

/* Initialize first screen */
showScreen(SCREEN_MENU);
</script>
