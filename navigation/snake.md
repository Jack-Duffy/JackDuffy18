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

# Snake Game Code

Copy and paste the following sections into your `snake.md` file.

---

## Part 1: HTML and CSS
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Snake Game</title>
  <style>
    /* Body Styling */
    body {
      margin: 0;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100vh;
      font-family: Arial, sans-serif;
      background-color: #f0f0f0;
    }

    /* Canvas Styling */
    canvas {
      border: 10px solid darkgreen;
      background: repeating-linear-gradient(
          0deg,
          lightgreen 20px,
          green 20px,
          green 40px
        ),
        repeating-linear-gradient(
          90deg,
          lightgreen 20px,
          green 20px,
          green 40px
        );
    }

    /* Buttons Styling */
    button {
      margin: 10px;
      padding: 10px 20px;
      font-size: 16px;
      cursor: pointer;
    }

    /* Hidden Screens */
    #setting, #gameover {
      display: none;
    }
  </style>
</head>
<body>
  <!-- Menu Screen -->
  <div id="menu">
    <button id="new_game">New Game</button>
    <button id="setting_menu">Settings</button>
  </div>

  <!-- Settings Screen -->
  <div id="setting">
    <h3>Settings</h3>
    <label>
      Wall Collisions:
      <input type="radio" name="wall" value="true" checked> On
      <input type="radio" name="wall" value="false"> Off
    </label>
    <br>
    <label>
      Speed:
      <input type="radio" name="speed" value="200" checked> Easy
      <input type="radio" name="speed" value="100"> Medium
      <input type="radio" name="speed" value="50"> Hard
    </label>
    <br>
    <button id="new_game1">Start Game</button>
  </div>

  <!-- Game Over Screen -->
  <div id="gameover">
    <h3>Game Over</h3>
    <p>Score: <span id="score_value">0</span></p>
    <button id="new_game2">Play Again</button>
  </div>

  <!-- Game Canvas -->
  <canvas id="snake" width="400" height="400"></canvas>
</body>
<script>
/* Game Attributes */
/////////////////////////////////////////////////////////////
// Canvas & Context
const canvas = document.getElementById("snake");
const ctx = canvas.getContext("2d");

// HTML Elements
const SCREEN_MENU = -1, SCREEN_SNAKE = 0, SCREEN_GAME_OVER = 1, SCREEN_SETTING = 2;
const menuScreen = document.getElementById("menu");
const settingScreen = document.getElementById("setting");
const gameOverScreen = document.getElementById("gameover");
const scoreDisplay = document.getElementById("score_value");

// Buttons
const startButtons = [document.getElementById("new_game"), document.getElementById("new_game1"), document.getElementById("new_game2")];
const settingsButton = document.getElementById("setting_menu");

// Game Settings
let wallCollision = true;
let speed = 200;

// Game State
let snake = [];
let direction = { x: 0, y: 0 };
let food = { x: 0, y: 0, color: "red" };
let score = 0;
let interval;

// Utility Functions
function showScreen(screen) {
  menuScreen.style.display = screen === SCREEN_MENU ? "block" : "none";
  settingScreen.style.display = screen === SCREEN_SETTING ? "block" : "none";
  gameOverScreen.style.display = screen === SCREEN_GAME_OVER ? "block" : "none";
  canvas.style.display = screen === SCREEN_SNAKE ? "block" : "none";
}

function generateFood() {
  const colors = ["red", "orange", "yellow"];
  food.x = Math.floor(Math.random() * (canvas.width / 20)) * 20;
  food.y = Math.floor(Math.random() * (canvas.height / 20)) * 20;
  food.color = colors[Math.floor(Math.random() * colors.length)];
}
/* Initialize Game */
function initializeGame() {
  snake = [{ x: 200, y: 200 }];
  direction = { x: 0, y: 0 };
  score = 0;
  generateFood();
  showScreen(SCREEN_SNAKE);
  clearInterval(interval);
  interval = setInterval(gameLoop, speed);
}

/* Game Loop */
function gameLoop() {
  // Move the snake
  const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };
  snake.unshift(head);

  // Check food collision
  if (head.x === food.x && head.y === food.y) {
    score++;
    generateFood();
  } else {
    snake.pop();
  }

  // Check wall collisions
  if (wallCollision && (head.x < 0 || head.y < 0 || head.x >= canvas.width || head.y >= canvas.height)) {
    endGame();
    return;
  }

  // Check self-collision
  if (snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y)) {
    endGame();
    return;
  }

  // Render game
  renderGame();
}

/* Render Game */
function renderGame() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // Draw food
  ctx.fillStyle = food.color;
  ctx.fillRect(food.x, food.y, 20, 20);

  // Draw snake
  snake.forEach((segment, index) => {
    ctx.fillStyle = "blue";
    ctx.fillRect(segment.x, segment.y, 20, 20);

    if (index === 0) {
      // Draw eyes on the head
      ctx.fillStyle = "white";
      ctx.fillRect(segment.x + 5, segment.y + 5, 5, 5);
      ctx.fillRect(segment.x + 10, segment.y + 5, 5, 5);
    }
  });
}

/* End Game */
function endGame() {
  clearInterval(interval);
  showScreen(SCREEN_GAME_OVER);
  scoreDisplay.textContent = score;
}

/* Input Handling */
document.addEventListener("keydown", e => {
  if (e.key === "ArrowUp" || e.key === "w") direction = { x: 0, y: -20 };
  if (e.key === "ArrowDown" || e.key === "s") direction = { x: 0, y: 20 };
  if (e.key === "ArrowLeft" || e.key === "a") direction = { x: -20, y: 0 };
  if (e.key === "ArrowRight" || e.key === "d") direction = { x: 20, y: 0 };
});

/* Event Listeners */
startButtons.forEach(button => button.onclick = initializeGame);
settingsButton.onclick = () => showScreen(SCREEN_SETTING);

/* Default Screen */
showScreen(SCREEN_MENU);
</script>
