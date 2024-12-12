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



<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Colorful Snake Game</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            color: #fff;
            text-align: center;
            transition: all 0.3s ease;
        }
        .container {
            position: relative;
            width: 400px;
            height: 400px;
            background: #34495e;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.6);
            overflow: hidden;
        }
        #home-page {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            height: 100%;
        }
        h1 {
            font-size: 36px;
            margin-bottom: 20px;
            color: #e74c3c;
        }
        p {
            font-size: 20px;
            margin-bottom: 20px;
            color: #fff;
        }
        .button {
            padding: 15px 30px;
            font-size: 18px;
            background-color: #1abc9c;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .button:hover {
            background-color: #16a085;
        }
        .game-container {
            display: none;
            position: relative;
            width: 100%;
            height: 100%;
        }
        .snake {
            position: absolute;
            width: 20px;
            height: 20px;
            background: linear-gradient(45deg, #ff6f61, #f39c12);
            border-radius: 5px; /* Rounded squares */
        }
        .apple {
            position: absolute;
            width: 20px;
            height: 20px;
            background-color: #9b59b6;
            border-radius: 50%;
        }
        #game-over {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            font-size: 24px;
            color: #fff;
            text-align: center;
            display: none;
        }
        #game-over p {
            margin-bottom: 20px;
        }
        #score {
            position: absolute;
            top: 10px;
            left: 10px;
            font-size: 20px;
            color: #fff;
            background-color: rgba(0, 0, 0, 0.5);
            padding: 10px;
            border-radius: 8px;
        }
    </style>
</head>
<body>
    <div class="container">
        <!-- Home Page -->
        <div id="home-page">
            <h1>Colorful Snake Game</h1>
            <p>Use <strong>WASD</strong> keys to control the snake.</p>
            <button class="button" onclick="startGame()">Start Game</button>
        </div>

        <!-- Game Area -->
        <div class="game-container" id="game">
            <div id="score">Score: 0</div>
            <div id="game-over">
                <p>Game Over! Your Score: <span id="final-score"></span></p>
                <button class="button" onclick="restartGame()">Restart</button>
            </div>
        </div>
    </div>

    <script>
        const gameContainer = document.querySelector('.game-container');
        const homePage = document.getElementById('home-page');
        const gameOverScreen = document.getElementById('game-over');
        const scoreDisplay = document.getElementById('score');
        const finalScoreDisplay = document.getElementById('final-score');
        const tileSize = 20;
        const gridSize = 20; // 20 x 20 grid

        let snake = [{ x: 5, y: 5 }];
        let direction = { x: 0, y: 0 };
        let apple = { x: 10, y: 10 };
        let score = 0;
        let isGameRunning = false;

        function startGame() {
            homePage.style.display = 'none';
            gameContainer.style.display = 'block';
            resetGame();
            isGameRunning = true;
            drawGame();
        }

        function restartGame() {
            resetGame();
            gameOverScreen.style.display = 'none';
            gameContainer.style.display = 'block';
            isGameRunning = true;
            drawGame();
        }

        function resetGame() {
            snake = [{ x: 5, y: 5 }];
            direction = { x: 1, y: 0 }; // Start moving right
            score = 0;
            placeApple();
            scoreDisplay.textContent = `Score: ${score}`;
        }

        function placeApple() {
            apple.x = Math.floor(Math.random() * gridSize);
            apple.y = Math.floor(Math.random() * gridSize);
        }

        function drawGame() {
            if (!isGameRunning) return;

            // Clear the game area
            gameContainer.innerHTML = '';
            gameContainer.appendChild(scoreDisplay);

            // Draw the snake
            snake.forEach(segment => {
                const snakeElement = document.createElement("div");
                snakeElement.style.left = `${segment.x * tileSize}px`;
                snakeElement.style.top = `${segment.y * tileSize}px`;
                snakeElement.classList.add("snake");
                gameContainer.appendChild(snakeElement);
            });

            // Draw the apple
            const appleElement = document.createElement("div");
            appleElement.style.left = `${apple.x * tileSize}px`;
            appleElement.style.top = `${apple.y * tileSize}px`;
            appleElement.classList.add("apple");
            gameContainer.appendChild(appleElement);

            // Move the snake
            moveSnake();

            // Check for collisions
            if (checkCollision()) {
                gameOver();
                return;
            }

            // Continue the game
            setTimeout(drawGame, 150);
        }

        function moveSnake() {
            const newHead = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

            // Check if the snake eats the apple
            if (newHead.x === apple.x && newHead.y === apple.y) {
                placeApple();
                score++;
                scoreDisplay.textContent = `Score: ${score}`;
            } else {
                snake.pop(); // Remove the tail
            }

            snake.unshift(newHead); // Add new head
        }

        function checkCollision() {
            // Check wall collision
            if (
                snake[0].x < 0 ||
                snake[0].x >= gridSize ||
                snake[0].y < 0 ||
                snake[0].y >= gridSize
            ) {
                return true;
            }

            // Check self-collision
            for (let i = 1; i < snake.length; i++) {
                if (snake[i].x === snake[0].x && snake[i].y === snake[0].y) {
                    return true;
                }
            }

            return false;
        }

        function gameOver() {
            isGameRunning = false;
            gameContainer.style.display = 'none';
            finalScoreDisplay.textContent = score;
            gameOverScreen.style.display = 'block';
        }

        // Event listeners
        document.body.addEventListener("keydown", (event) => {
            if (event.key === "w" && direction.y === 0) {
                direction = { x: 0, y: -1 };
            } else if (event.key === "s" && direction.y === 0) {
                direction = { x: 0, y: 1 };
            } else if (event.key === "a" && direction.x === 0) {
                direction = { x: -1, y: 0 };
            } else if (event.key === "d" && direction.x === 0) {
                direction = { x: 1, y: 0 };
            }
            // Hotkey for restart
            if (event.key === "r" && !isGameRunning) {
                restartGame();
            }
        });
    </script>
</body>
</html>


___________________________________________________________________________________________________________________________