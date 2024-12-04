---
layout: base
title: Student Home
description: Home Page
hide: true
---

Who am I?

My Name is jack Duffy and some of my favorite things to do are snowboarding and playing video games. I hope to become an engineer and eventually a buisness owner. Some of my goals in this class are yo learn how to code so I can put my 3d animation skill together with coding and create video games.

![alt text](<images/IMG_5269 copy.JPG>)
This is my Dog Named Piper


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #f0f0f0;
        }
        canvas {
            border: 2px solid black;
            background-color: #e2e2e2;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        const gridSize = 20; // Size of each grid cell
        const rows = canvas.height / gridSize;
        const cols = canvas.width / gridSize;

        let snake = [{ x: 10, y: 10 }]; // Snake starts with one segment
        let food = { x: Math.floor(Math.random() * cols), y: Math.floor(Math.random() * rows) };
        let direction = { x: 0, y: 0 }; // Current movement direction
        let score = 0;

        function drawGame() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // Draw the snake
            ctx.fillStyle = "green";
            snake.forEach(segment => {
                ctx.fillRect(segment.x * gridSize, segment.y * gridSize, gridSize, gridSize);
            });

            // Draw the food
            ctx.fillStyle = "red";
            ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize, gridSize);

            // Move the snake
            const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

            // Check if the snake eats the food
            if (head.x === food.x && head.y === food.y) {
                score++;
                food = { x: Math.floor(Math.random() * cols), y: Math.floor(Math.random() * rows) };
            } else {
                snake.pop(); // Remove the tail if no food eaten
            }

            // Add the new head
            snake.unshift(head);

            // Check for collision with walls or itself
            if (
                head.x < 0 || head.y < 0 ||
                head.x >= cols || head.y >= rows ||
                snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y)
            ) {
                alert(`Game Over! Your score: ${score}`);
                snake = [{ x: 10, y: 10 }];
                direction = { x: 0, y: 0 };
                score = 0;
            }

            requestAnimationFrame(drawGame);
        }

        // Change direction with arrow keys
        window.addEventListener("keydown", (event) => {
            switch (event.key) {
                case "ArrowUp":
                    if (direction.y === 0) direction = { x: 0, y: -1 };
                    break;
                case "ArrowDown":
                    if (direction.y === 0) direction = { x: 0, y: 1 };
                    break;
                case "ArrowLeft":
                    if (direction.x === 0) direction = { x: -1, y: 0 };
                    break;
                case "ArrowRight":
                    if (direction.x === 0) direction = { x: 1, y: 0 };
                    break;
            }
        });

        drawGame();
    </script>
</body>
</html>
