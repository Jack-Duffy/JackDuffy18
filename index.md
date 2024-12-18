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
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background: #111;
            color: #fff;
            font-family: Arial, sans-serif;
        }
        canvas {
            background-color: #111;
            display: block;
        }
        #menu, #game-over {
            display: none;
            text-align: center;
        }
    </style>
</head>
<body>
    <div id="menu">
        <h1>Snake Game</h1>
        <button id="start-game">Start Game</button>
    </div>
    <div id="game-over">
        <h1>Game Over</h1>
        <p>Score: <span id="final-score">0</span></p>
        <button id="restart-game">Restart Game</button>
    </div>
    <canvas id="game-canvas" width="400" height="400" tabindex="0"></canvas>
    <script>
        (() => {
            const canvas = document.getElementById('game-canvas');
            const ctx = canvas.getContext('2d');
            const menu = document.getElementById('menu');
            const gameOverScreen = document.getElementById('game-over');
            const startButton = document.getElementById('start-game');
            const restartButton = document.getElementById('restart-game');
            const finalScore = document.getElementById('final-score');
            const BLOCK = 20;
            const rows = canvas.height / BLOCK;
            const cols = canvas.width / BLOCK;
            let snake = [];
            let food = {};
            let direction = { x: 1, y: 0 };
            let nextDirection = { x: 1, y: 0 };
            let score = 0;
            let gameRunning = false;
            /* Utility Functions */
            /////////////////////////////////////////////////////////////
            const drawCheckerboard = () => {
                for (let y = 0; y < rows; y++) {
                    for (let x = 0; x < cols; x++) {
                        ctx.fillStyle = (x + y) % 2 === 0 ? "#006400" : "#008000"; // Checkerboard shades of green
                        ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
                    }
                }
            };
            const drawBlock = (x, y, color) => {
                ctx.fillStyle = color;
                ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
            };
            const randomPosition = () => {
                return {
                    x: Math.floor(Math.random() * cols),
                    y: Math.floor(Math.random() * rows),
                };
            };
            const resetGame = () => {
                snake = [{ x: 5, y: 5 }];
                direction = { x: 1, y: 0 };
                nextDirection = { x: 1, y: 0 };
                score = 0;
                food = randomPosition();
                gameRunning = true;
                menu.style.display = "none";
                gameOverScreen.style.display = "none";
                canvas.focus();
            };
            const updateScore = () => {
                finalScore.textContent = score;
            };
            /* Game Logic */
            /////////////////////////////////////////////////////////////
            const update = () => {
                if (!gameRunning) return;
                const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };
                // Wrap-around behavior
                if (head.x < 0) head.x = cols - 1;
                if (head.y < 0) head.y = rows - 1;
                if (head.x >= cols) head.x = 0;
                if (head.y >= rows) head.y = 0;
                // Check collision with the snake body
                for (let i = 0; i < snake.length; i++) {
                    if (snake[i].x === head.x && snake[i].y === head.y) {
                        endGame();
                        return;
                    }
                }
                snake.unshift(head);
                // Check if the snake eats the food
                if (head.x === food.x && head.y === food.y) {
                    score++;
                    updateScore();
                    food = randomPosition();
                } else {
                    snake.pop();
                }
                // Update direction
                direction = nextDirection;
                render();
            };
            const render = () => {
                drawCheckerboard();
                // Draw food
                drawBlock(food.x, food.y, "#FF0000"); // Red food
                // Draw snake
                snake.forEach((segment) => {
                    drawBlock(segment.x, segment.y, "#0000FF"); // Blue snake
                });
            };
            const endGame = () => {
                gameRunning = false;
                gameOverScreen.style.display = "block";
                menu.style.display = "none";
            };
            /* Event Listeners */
            /////////////////////////////////////////////////////////////
            document.addEventListener("keydown", (e) => {
                if (!gameRunning) return;
                switch (e.key) {
                    case "ArrowUp":
                        if (direction.y === 0) nextDirection = { x: 0, y: -1 };
                        break;
                    case "ArrowDown":
                        if (direction.y === 0) nextDirection = { x: 0, y: 1 };
                        break;
                    case "ArrowLeft":
                        if (direction.x === 0) nextDirection = { x: -1, y: 0 };
                        break;
                    case "ArrowRight":
                        if (direction.x === 0) nextDirection = { x: 1, y: 0 };
                        break;
                }
            });
            startButton.addEventListener("click", () => {
                resetGame();
                gameLoop();
            });
            restartButton.addEventListener("click", () => {
                resetGame();
                gameLoop();
            });
            const gameLoop = () => {
                if (!gameRunning) return;
                update();
                setTimeout(gameLoop, 100);
            };
            /* Initialize */
            /////////////////////////////////////////////////////////////
            menu.style.display = "block";
        })();
            /* Dynamic Speed Adjustment */
            /////////////////////////////////////////////////////////////
            let speed = 100; // Default speed (milliseconds)
            const adjustSpeed = (newSpeed) => {
                speed = newSpeed;
            };
            const speedSettings = () => {
                const fastSpeed = document.getElementById('fast-speed');
                const normalSpeed = document.getElementById('normal-speed');
                fastSpeed.onclick = () => adjustSpeed(70); // Faster snake
                normalSpeed.onclick = () => adjustSpeed(100); // Default speed
            };
            /* Pause Feature */
            /////////////////////////////////////////////////////////////
            let isPaused = false;
            const togglePause = () => {
                isPaused = !isPaused;
                if (!isPaused) {
                    gameLoop(); // Resume game loop if unpaused
                }
            };
            document.addEventListener('keydown', (e) => {
                if (e.key === ' ') {
                    togglePause();
                }
            });
            /* Score Display */
            /////////////////////////////////////////////////////////////
            const displayScore = () => {
                const scoreDisplay = document.createElement('div');
                scoreDisplay.id = 'score-display';
                scoreDisplay.style.position = 'absolute';
                scoreDisplay.style.top = '10px';
                scoreDisplay.style.left = '10px';
                scoreDisplay.style.color = '#fff';
                scoreDisplay.style.fontSize = '18px';
                scoreDisplay.textContent = `Score: ${score}`;
                document.body.appendChild(scoreDisplay);
                const updateScoreDisplay = () => {
                    const scoreDiv = document.getElementById('score-display');
                    if (scoreDiv) scoreDiv.textContent = `Score: ${score}`;
                };
                return updateScoreDisplay;
            };
            const updateScoreDisplay = displayScore();
            /* Settings Menu */
            /////////////////////////////////////////////////////////////
            const settingsMenu = () => {
                const settingsDiv = document.createElement('div');
                settingsDiv.id = 'settings-menu';
                settingsDiv.style.position = 'absolute';
                settingsDiv.style.bottom = '10px';
                settingsDiv.style.left = '10px';
                settingsDiv.style.backgroundColor = '#222';
                settingsDiv.style.padding = '10px';
                settingsDiv.style.borderRadius = '5px';
                const fastButton = document.createElement('button');
                fastButton.id = 'fast-speed';
                fastButton.textContent = 'Fast Speed';
                settingsDiv.appendChild(fastButton);
                const normalButton = document.createElement('button');
                normalButton.id = 'normal-speed';
                normalButton.textContent = 'Normal Speed';
                settingsDiv.appendChild(normalButton);
                document.body.appendChild(settingsDiv);
                speedSettings();
            };
            settingsMenu();
            /* Enhanced Food Behavior */
            /////////////////////////////////////////////////////////////
            const enhancedFoodLogic = () => {
                let specialFood = null;
                const addSpecialFood = () => {
                    if (Math.random() < 0.1) { // 10% chance to spawn special food
                        specialFood = randomPosition();
                    }
                };
                const drawSpecialFood = () => {
                    if (specialFood) {
                        drawBlock(specialFood.x, specialFood.y, '#FFD700'); // Gold for special food
                    }
                };
                const handleSpecialFoodCollision = (head) => {
                    if (specialFood && head.x === specialFood.x && head.y === specialFood.y) {
                        score += 5; // Bonus points
                        updateScoreDisplay();
                        specialFood = null;
                    }
                };
                return { addSpecialFood, drawSpecialFood, handleSpecialFoodCollision };
            };
            const { addSpecialFood, drawSpecialFood, handleSpecialFoodCollision } = enhancedFoodLogic();
            /* Game Loop Update with Enhanced Logic */
            /////////////////////////////////////////////////////////////
            const gameLoop = () => {
                if (!gameRunning || isPaused) return;
                update();
                const head = snake[0];
                handleSpecialFoodCollision(head);
                addSpecialFood();
                render();
                drawSpecialFood();
                setTimeout(gameLoop, speed);
            };
            /* Enhanced Game Initialization */
            /////////////////////////////////////////////////////////////
            const initGame = () => {
                resetGame();
                // Remove existing UI elements to avoid duplicates
                const existingScoreDisplay = document.getElementById('score-display');
                const existingSettingsMenu = document.getElementById('settings-menu');
                if (existingScoreDisplay) existingScoreDisplay.remove();
                if (existingSettingsMenu) existingSettingsMenu.remove();
                // Reinitialize UI elements
                displayScore();
                settingsMenu();
                gameLoop();
            };
            startButton.addEventListener('click', initGame);
            restartButton.addEventListener('click', initGame);
        })();
    </script>
</body>
</html>
