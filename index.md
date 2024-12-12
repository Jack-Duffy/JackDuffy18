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
    <title>Advanced Snake Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            background-color: #f0f0f0;
        }
        #game-container {
            text-align: center;
            margin: auto;
            width: 400px;
            height: 400px;
            position: relative;
            border: 5px solid #000;
            background-color: #d4f1f9;
        }
        .snake {
            position: absolute;
            width: 20px;
            height: 20px;
            background-color: #4CAF50;
            border-radius: 5px;
        }
        .apple {
            position: absolute;
            width: 20px;
            height: 20px;
            background-color: #FF5733;
            border-radius: 50%;
        }
        #game-over {
            display: none;
            font-size: 24px;
            color: black;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background-color: rgba(255, 255, 255, 0.9);
            padding: 20px;
            border-radius: 10px;
        }
        #restart-btn {
            margin-top: 10px;
            padding: 10px 20px;
            font-size: 18px;
            background-color: #007BFF;
            color: white;
            border: none;
            border-radius: 5px;
            cursor: pointer;
        }
        #restart-btn:hover {
            background-color: #0056b3;
        }
    </style>
</head>
<body>
    <div id="game-container">
        <div id="game"></div>
        <div id="game-over">
            <p>Game Over!</p>
            <button id="restart-btn">Restart</button>
        </div>
    </div>

    <script>
        const gameContainer = document.getElementById("game-container");
        const game = document.getElementById("game");
        const gameOverScreen = document.getElementById("game-over");
        const restartButton = document.getElementById("restart-btn");
        const tileSize = 20;
        const gridSize = 20;

        let snake = [{ x: 5, y: 5 }];
        let direction = { x: 0, y: 0 };
        let apple = { x: 10, y: 10 };
        let isGameRunning = false;

        function startGame() {
            gameOverScreen.style.display = "none";
            game.innerHTML = "";
            snake = [{ x: 5, y: 5 }];
            direction = { x: 1, y: 0 };
            placeApple();
            isGameRunning = true;
            drawGame();
        }

        function placeApple() {
            apple.x = Math.floor(Math.random() * gridSize);
            apple.y = Math.floor(Math.random() * gridSize);
        }

        function drawGame() {
            if (!isGameRunning) return;

            game.innerHTML = "";

            // Draw snake
            snake.forEach(segment => {
                const snakeElement = document.createElement("div");
                snakeElement.style.left = `${segment.x * tileSize}px`;
                snakeElement.style.top = `${segment.y * tileSize}px`;
                snakeElement.classList.add("snake");
                game.appendChild(snakeElement);
            });

            // Draw apple
            const appleElement = document.createElement("div");
            appleElement.style.left = `${apple.x * tileSize}px`;
            appleElement.style.top = `${apple.y * tileSize}px`;
            appleElement.classList.add("apple");
            game.appendChild(appleElement);

            moveSnake();

            if (checkCollision()) {
                endGame();
                return;
            }

            setTimeout(drawGame, 150);
        }

        function moveSnake() {
            const newHead = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

            if (newHead.x === apple.x && newHead.y === apple.y) {
                placeApple();
            } else {
                snake.pop();
            }

            snake.unshift(newHead);
        }

        function checkCollision() {
            const head = snake[0];
            if (
                head.x < 0 || head.x >= gridSize ||
                head.y < 0 || head.y >= gridSize
            ) {
                return true;
            }

            for (let i = 1; i < snake.length; i++) {
                if (snake[i].x === head.x && snake[i].y === head.y) {
                    return true;
                }
            }

            return false;
        }

        function endGame() {
            isGameRunning = false;
            gameOverScreen.style.display = "block";
        }

        document.addEventListener("keydown", (event) => {
            if (!isGameRunning) return;

            switch (event.key) {
                case "w":
                    if (direction.y === 0) direction = { x: 0, y: -1 };
                    break;
                case "s":
                    if (direction.y === 0) direction = { x: 0, y: 1 };
                    break;
                case "a":
                    if (direction.x === 0) direction = { x: -1, y: 0 };
                    break;
                case "d":
                    if (direction.x === 0) direction = { x: 1, y: 0 };
                    break;
                default:
                    break;
            }
        });

        restartButton.addEventListener("click", startGame);

        // Start the game on load
        startGame();
    </script>
</body>
</html>



___________________________________________________________________________________________________________________________