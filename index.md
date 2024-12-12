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
            margin: 0;
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #e8f5e9;
        }
        canvas {
            display: none;
            background: linear-gradient(to bottom, #8bc34a, #689f38);
            border: 10px solid #33691e;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.5);
        }
        .menu {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
        }
        .menu h1 {
            font-size: 3rem;
            color: #33691e;
            margin-bottom: 20px;
        }
        .menu p {
            font-size: 1.2rem;
            margin-bottom: 20px;
        }
        .menu button {
            padding: 10px 20px;
            font-size: 1rem;
            background-color: #689f38;
            color: white;
            border: none;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .menu button:hover {
            background-color: #33691e;
        }
    </style>
</head>
<body>
    <div class="menu" id="startMenu">
        <h1>Snake Game</h1>
        <p>Use WASD to control the snake. Collect apples to grow longer. Avoid the edges and yourself!</p>
        <button onclick="startGame()">Start Game</button>
    </div>
    <div class="menu" id="endMenu" style="display: none;">
        <h1 id="endMessage"></h1>
        <button onclick="restartGame()">Restart Game</button>
    </div>
    <canvas id="gameCanvas" width="400" height="400"></canvas>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');
        const startMenu = document.getElementById('startMenu');
        const endMenu = document.getElementById('endMenu');
        const endMessage = document.getElementById('endMessage');

        const gridSize = 20;
        let snake = [{ x: 5, y: 5 }];
        let direction = { x: 0, y: 0 };
        let apple = { x: 10, y: 10 };
        let gameRunning = false;
        
        function startGame() {
            startMenu.style.display = 'none';
            canvas.style.display = 'block';
            snake = [{ x: 5, y: 5 }];
            direction = { x: 0, y: 0 };
            placeApple();
            gameRunning = true;
            gameLoop();
        }

        function endGame(message) {
            gameRunning = false;
            endMessage.textContent = message;
            endMenu.style.display = 'flex';
            canvas.style.display = 'none';
        }

        function restartGame() {
            endMenu.style.display = 'none';
            startGame();
        }

        function placeApple() {
            apple.x = Math.floor(Math.random() * (canvas.width / gridSize));
            apple.y = Math.floor(Math.random() * (canvas.height / gridSize));
        }

        function gameLoop() {
            if (!gameRunning) return;

            // Update snake position
            const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

            // Check collisions
            if (head.x < 0 || head.y < 0 || head.x >= canvas.width / gridSize || head.y >= canvas.height / gridSize ||
                snake.some(segment => segment.x === head.x && segment.y === head.y)) {
                endGame('Game Over');
                return;
            }

            snake.unshift(head);

            // Check if apple is eaten
            if (head.x === apple.x && head.y === apple.y) {
                placeApple();
            } else {
                snake.pop();
            }

            // Draw the game
            drawGame();

            setTimeout(gameLoop, 100);
        }

        function drawGame() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw checkerboard background
            for (let x = 0; x < canvas.width / gridSize; x++) {
                for (let y = 0; y < canvas.height / gridSize; y++) {
                    ctx.fillStyle = (x + y) % 2 === 0 ? '#8bc34a' : '#689f38';
                    ctx.fillRect(x * gridSize, y * gridSize, gridSize, gridSize);
                }
            }

            // Draw apple
            ctx.fillStyle = 'red';
            ctx.beginPath();
            ctx.arc(apple.x * gridSize + gridSize / 2, apple.y * gridSize + gridSize / 2, gridSize / 3, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = 'brown';
            ctx.fillRect(apple.x * gridSize + gridSize / 2 - 2, apple.y * gridSize + gridSize / 2 - gridSize / 2, 4, 6);

            // Draw snake
            for (let i = 0; i < snake.length; i++) {
                const gradient = ctx.createLinearGradient(0, 0, canvas.width, canvas.height);
                gradient.addColorStop(0, `rgba(33, 150, 243, ${1 - i / snake.length})`);
                gradient.addColorStop(1, `rgba(3, 169, 244, ${1 - i / snake.length})`);
                ctx.fillStyle = gradient;
                ctx.fillRect(snake[i].x * gridSize, snake[i].y * gridSize, gridSize, gridSize);

                if (i === 0) {
                    ctx.fillStyle = 'black';
                    ctx.beginPath();
                    ctx.arc(snake[i].x * gridSize + gridSize / 3, snake[i].y * gridSize + gridSize / 3, 2, 0, Math.PI * 2);
                    ctx.arc(snake[i].x * gridSize + gridSize * 2 / 3, snake[i].y * gridSize + gridSize / 3, 2, 0, Math.PI * 2);
                    ctx.fill();
                }
            }
        }

        window.addEventListener('keydown', (e) => {
            if (!gameRunning) return;
            switch (e.key) {
                case 'w':
                    if (direction.y === 0) direction = { x: 0, y: -1 };
                    break;
                case 'a':
                    if (direction.x === 0) direction = { x: -1, y: 0 };
                    break;
                case 's':
                    if (direction.y === 0) direction = { x: 0, y: 1 };
                    break;
                case 'd':
                    if (direction.x === 0) direction = { x: 1, y: 0 };
                    break;
            }
        });
    </script>
</body>
</html>



___________________________________________________________________________________________________________________________