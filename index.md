---
layout: base
title: Student Home
description: Home Page
hide: true
---
-
# About Me 
Not in About(Dont worry about it)


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Intermediate Snake Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        #game-container {
            text-align: center;
        }
        #start-screen, #game-over, #win-screen {
            display: none;
            font-size: 24px;
        }
        #game-over {
            color: red;
        }
        #win-screen {
            color: green;
        }
        #game {
            display: none;
            position: relative;
            width: 400px;
            height: 400px;
            background-image: url('https://via.placeholder.com/400x400?text=Game+Background');
            background-size: cover;
            border: 2px solid #fff;
            overflow: hidden;
        }
        .snake {
            position: absolute;
            width: 20px;
            height: 20px;
            background-image: url('https://via.placeholder.com/20/008000?text=');
            background-size: cover;
        }
        .apple {
            position: absolute;
            width: 20px;
            height: 20px;
            background-image: url('https://via.placeholder.com/20/FF0000?text=');
            background-size: cover;
        }
    </style>
</head>
<body>
    <div id="game-container">
        <div id="start-screen">
            <p>Welcome to Snake Game!<br>Click anywhere to start.</p>
        </div>
        <div id="game"></div>
        <div id="game-over">
            <p>Game Over! Click to Restart</p>
        </div>
        <div id="win-screen">
            <p>Congratulations! You Win! Click to Play Again</p>
        </div>
    </div>

    <script>
        const startScreen = document.getElementById("start-screen");
        const gameOverScreen = document.getElementById("game-over");
        const winScreen = document.getElementById("win-screen");
        const game = document.getElementById("game");
        const tileSize = 20;
        const gridSize = 20; // 20 x 20 grid

        let snake = [{ x: 5, y: 5 }];
        let direction = { x: 0, y: 0 };
        let apple = { x: 10, y: 10 };
        let isGameRunning = false;

        function startGame() {
            startScreen.style.display = "none";
            gameOverScreen.style.display = "none";
            winScreen.style.display = "none";
            game.style.display = "block";
            snake = [{ x: 5, y: 5 }];
            direction = { x: 1, y: 0 }; // Start moving right
            placeApple();
            isGameRunning = true;
            drawGame();
        }

        function restartGame() {
            isGameRunning = false;
            startScreen.style.display = "block";
            game.style.display = "none";
        }

        function placeApple() {
            apple.x = Math.floor(Math.random() * gridSize);
            apple.y = Math.floor(Math.random() * gridSize);
        }

        function drawGame() {
            if (!isGameRunning) return;

            // Clear the game area
            game.innerHTML = "";

            // Draw the snake
            snake.forEach(segment => {
                const snakeElement = document.createElement("div");
                snakeElement.style.left = `${segment.x * tileSize}px`;
                snakeElement.style.top = `${segment.y * tileSize}px`;
                snakeElement.classList.add("snake");
                game.appendChild(snakeElement);
            });

            // Draw the apple
            const appleElement = document.createElement("div");
            appleElement.style.left = `${apple.x * tileSize}px`;
            appleElement.style.top = `${apple.y * tileSize}px`;
            appleElement.classList.add("apple");
            game.appendChild(appleElement);

            // Move the snake
            moveSnake();

            // Check for collisions
            if (checkCollision()) {
                gameOver();
                return;
            }

            // Check if the player wins
            if (snake.length === gridSize * gridSize) {
                winGame();
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
            game.style.display = "none";
            gameOverScreen.style.display = "block";
        }

        function winGame() {
            isGameRunning = false;
            game.style.display = "none";
            winScreen.style.display = "block";
        }

        document.body.addEventListener("click", () => {
            if (!isGameRunning) {
                startGame();
            }
        });

        document.addEventListener("keydown", (event) => {
            if (event.key === "ArrowUp" && direction.y === 0) {
                direction = { x: 0, y: -1 };
            } else if (event.key === "ArrowDown" && direction.y === 0) {
                direction = { x: 0, y: 1 };
            } else if (event.key === "ArrowLeft" && direction.x === 0) {
                direction = { x: -1, y: 0 };
            } else if (event.key === "ArrowRight" && direction.x === 0) {
                direction = { x: 1, y: 0 };
            }
        });
    </script>
</body>
</html>
