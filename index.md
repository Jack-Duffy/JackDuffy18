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


let canvas = document.getElementById("canvas");
let ctx = canvas.getContext("2d");
let score = 0;
let snake = [{ x: 5, y: 5 }];
let food = { x: 10, y: 10, color: "#FF0000" }; // Default red apple
let snake_dir = 1; // Default direction: right
let snake_next_dir = 1;
let snake_speed = 150;
let wall = 1;
const BLOCK = 10;

/* Repaint canvas with a grid */
let repaintCanvas = function () {
    ctx.beginPath();
    const gridColor1 = "#228B22"; // Green
    const gridColor2 = "#32CD32"; // Light Green

    // Draw grid pattern
    for (let i = 0; i < canvas.width / BLOCK; i++) {
        for (let j = 0; j < canvas.height / BLOCK; j++) {
            ctx.fillStyle = (i + j) % 2 === 0 ? gridColor1 : gridColor2;
            ctx.fillRect(i * BLOCK, j * BLOCK, BLOCK, BLOCK);
        }
    }
};

/* Draw the snake */
let drawSnake = function () {
    for (let i = 0; i < snake.length; i++) {
        if (i === 0) {
            // Head with eyes
            ctx.fillStyle = "#0000FF"; // Blue
            ctx.fillRect(snake[i].x * BLOCK, snake[i].y * BLOCK, BLOCK, BLOCK);

            // Eyes
            ctx.fillStyle = "#FFFFFF"; // White
            const eyeSize = 2; // Eye size in pixels
            ctx.fillRect(snake[i].x * BLOCK + 3, snake[i].y * BLOCK + 3, eyeSize, eyeSize);
            ctx.fillRect(snake[i].x * BLOCK + 7, snake[i].y * BLOCK + 3, eyeSize, eyeSize);

            // Pupils
            ctx.fillStyle = "#000000"; // Black
            ctx.fillRect(snake[i].x * BLOCK + 4, snake[i].y * BLOCK + 4, 1, 1);
            ctx.fillRect(snake[i].x * BLOCK + 8, snake[i].y * BLOCK + 4, 1, 1);
        } else {
            // Body
            ctx.fillStyle = "#0000FF"; // Blue
            ctx.fillRect(snake[i].x * BLOCK, snake[i].y * BLOCK, BLOCK, BLOCK);
        }
    }
};

/* Draw the food */
let drawFood = function () {
    ctx.fillStyle = food.color;
    ctx.fillRect(food.x * BLOCK, food.y * BLOCK, BLOCK, BLOCK);
};

/* Random food placement and color */
let addFood = function () {
    food.x = Math.floor(Math.random() * (canvas.width / BLOCK));
    food.y = Math.floor(Math.random() * (canvas.height / BLOCK));

    // Avoid placing the food on the snake
    for (let i = 0; i < snake.length; i++) {
        if (checkBlock(food.x, food.y, snake[i].x, snake[i].y)) {
            addFood();
            return;
        }
    }

    // Randomize food color
    const colors = ["#FF0000", "#008000", "#FFFF00", "#FFA500"]; // Red, Green, Yellow, Orange
    food.color = colors[Math.floor(Math.random() * colors.length)];
};

/* Check if two blocks are at the same position */
let checkBlock = function (x, y, _x, _y) {
    return x === _x && y === _y;
};

/* Update score display */
let altScore = function (score) {
    document.getElementById("score_value").innerHTML = String(score);
};

/* Main game loop */
let mainLoop = function () {
    let _x = snake[0].x;
    let _y = snake[0].y;
    snake_dir = snake_next_dir;

    // Update head position
    switch (snake_dir) {
        case 0: _y--; break; // Up
        case 1: _x++; break; // Right
        case 2: _y++; break; // Down
        case 3: _x--; break; // Left
    }

    // Update snake body
    snake.pop();
    snake.unshift({ x: _x, y: _y });

    // Wall collision (if enabled)
    if (wall === 1) {
        if (_x < 0 || _x >= canvas.width / BLOCK || _y < 0 || _y >= canvas.height / BLOCK) {
            showScreen(SCREEN_GAME_OVER);
            return;
        }
    } else {
        // Wrap around behavior
        if (_x < 0) _x = canvas.width / BLOCK - 1;
        if (_x >= canvas.width / BLOCK) _x = 0;
        if (_y < 0) _y = canvas.height / BLOCK - 1;
        if (_y >= canvas.height / BLOCK) _y = 0;
    }

    // Self-collision
    for (let i = 1; i < snake.length; i++) {
        if (checkBlock(_x, _y, snake[i].x, snake[i].y)) {
            showScreen(SCREEN_GAME_OVER);
            return;
        }
    }

    // Check food collision
    if (checkBlock(_x, _y, food.x, food.y)) {
        snake.push({ x: food.x, y: food.y });
        altScore(++score);
        addFood();
    }

    // Repaint the canvas
    repaintCanvas();
    drawSnake();
    drawFood();

    // Call next frame
    setTimeout(mainLoop, snake_speed);
};

/* Handle keyboard inputs */
window.addEventListener("keydown", function (e) {
    const key = e.key.toLowerCase();
    switch (key) {
        case "w": // Up
            if (snake_dir !== 2) snake_next_dir = 0;
            break;
        case "d": // Right
            if (snake_dir !== 3) snake_next_dir = 1;
            break;
        case "s": // Down
            if (snake_dir !== 0) snake_next_dir = 2;
            break;
        case "a": // Left
            if (snake_dir !== 1) snake_next_dir = 3;
            break;
    }
});

/* Start the game */
let startGame = function () {
    showScreen(SCREEN_DEFAULT);
    repaintCanvas();
    drawSnake();
    drawFood();
    mainLoop();
};


___________________________________________________________________________________________________________________________