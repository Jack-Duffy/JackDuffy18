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


---
layout: base
title: Snake
permalink: /snake/
---

<style>
    body {}
    .wrap {
        margin: auto;
    }
    canvas {
        display: none;
        border: solid 10px #FFFFFF;
    }
    canvas:focus {
        outline: none;
    }
    #gameover p, #setting p, #menu p {
        font-size: 20px;
    }
    #gameover a, #setting a, #menu a {
        font-size: 30px;
        display: block;
        cursor: pointer;
    }
    #menu {
        display: block;
    }
    #gameover, #setting {
        display: none;
    }
</style>

<h2>Snake</h2>
<div class="container">
    <header class="pb-3 border-bottom border-primary text-dark">
        <p>Score: <span id="score_value">0</span></p>
    </header>
    <div class="container bg-secondary" style="text-align:center;">
        <div id="menu" class="py-4 text-light">
            <p>Press <span style="background:#FFF;color:#000">space</span> to begin</p>
            <a id="new_game">new game</a>
            <a id="setting_menu">settings</a>
        </div>
        <div id="gameover" class="py-4 text-light">
            <p>Game Over, press <span style="background:#FFF;color:#000">space</span> to retry</p>
            <a id="new_game1">new game</a>
        </div>
        <canvas id="snake" class="wrap" width="640" height="640" tabindex="0"></canvas>
        <div id="setting" class="py-4 text-light">
            <p>Speed:
                <input id="speed1" type="radio" name="speed" value="120" checked/>
                <label for="speed1">Slow</label>
                <input id="speed2" type="radio" name="speed" value="75"/>
                <label for="speed2">Normal</label>
                <input id="speed3" type="radio" name="speed" value="35"/>
                <label for="speed3">Fast</label>
            </p>
        </div>
    </div>
</div>

<script>
(function() {
    // Constants
    const SCREEN_MENU = -1;
    const SCREEN_SNAKE = 0;
    const SCREEN_GAME_OVER = 1;
    const BLOCK = 10;

    // DOM Elements
    const canvas = document.getElementById("snake");
    const ctx = canvas.getContext("2d");
    const screen_menu = document.getElementById("menu");
    const screen_game_over = document.getElementById("gameover");
    const ele_score = document.getElementById("score_value");
    const button_new_game = document.getElementById("new_game");
    const button_new_game1 = document.getElementById("new_game1");

    // Game Variables
    let SCREEN = SCREEN_MENU;
    let snake = [], snake_dir = 1, snake_next_dir = 1;
    let food = {x: 0, y: 0, color: "#FF0000"};
    let score = 0;
    let snake_speed = 120;
    let game_running = false;

    // Helper: Show screen
    function showScreen(screen) {
        SCREEN = screen;
        screen_menu.style.display = (screen === SCREEN_MENU) ? "block" : "none";
        screen_game_over.style.display = (screen === SCREEN_GAME_OVER) ? "block" : "none";
        canvas.style.display = (screen === SCREEN_SNAKE) ? "block" : "none";
        if (screen === SCREEN_SNAKE) canvas.focus();
    }

    // Add food randomly
    function addFood() {
        food.x = Math.floor(Math.random() * (canvas.width / BLOCK));
        food.y = Math.floor(Math.random() * (canvas.height / BLOCK));
        food.color = "#" + Math.floor(Math.random() * 16777215).toString(16);
    }

    // Draw the game
    function draw() {
        ctx.fillStyle = "#000000";
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        snake.forEach(part => drawDot(part.x, part.y, "#FFFFFF"));
        drawDot(food.x, food.y, food.color);
    }

    function drawDot(x, y, color) {
        ctx.fillStyle = color;
        ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
    }

    // Update Snake Position
    function updateSnake() {
        let head = { ...snake[0] };
        switch (snake_next_dir) {
            case 0: head.y--; break; // Up
            case 1: head.x++; break; // Right
            case 2: head.y++; break; // Down
            case 3: head.x--; break; // Left
        }
        
        // Collision: Check walls
        if (head.x < 0 || head.x >= canvas.width / BLOCK || head.y < 0 || head.y >= canvas.height / BLOCK) {
            gameOver();
            return;
        }
        
        // Collision: Check snake body
        for (let i = 0; i < snake.length; i++) {
            if (snake[i].x === head.x && snake[i].y === head.y) {
                gameOver();
                return;
            }
        }
        
        snake.unshift(head);

        // Food Collision
        if (head.x === food.x && head.y === food.y) {
            score++;
            ele_score.innerText = score;
            addFood();
        } else {
            snake.pop();
        }
    }

    function gameOver() {
        game_running = false;
        showScreen(SCREEN_GAME_OVER);
    }

    function mainLoop() {
        if (!game_running) return;
        updateSnake();
        draw();
        setTimeout(mainLoop, snake_speed);
    }

    function newGame() {
        if (game_running) return;
        game_running = true;
        snake = [{x: 5, y: 5}];
        snake_dir = 1;
        snake_next_dir = 1;
        score = 0;
        ele_score.innerText = score;
        addFood();
        showScreen(SCREEN_SNAKE);
        mainLoop();
    }

    function changeDir(key) {
        switch (key) {
            case 37: if (snake_dir !== 1) snake_next_dir = 3; break; // Left
            case 38: if (snake_dir !== 2) snake_next_dir = 0; break; // Up
            case 39: if (snake_dir !== 3) snake_next_dir = 1; break; // Right
            case 40: if (snake_dir !== 0) snake_next_dir = 2; break; // Down
        }
    }

    window.onload = function() {
        button_new_game.onclick = newGame;
        button_new_game1.onclick = newGame;
        
        window.addEventListener("keydown", function(evt) {
            if (evt.code === "Space" && SCREEN !== SCREEN_SNAKE) newGame();
            changeDir(evt.keyCode);
        }, true);
    };
})();
</script>
        /* Snake Collision Detection */
        /////////////////////////////////////////////////////////////
        let checkBlock = function(x, y, _x, _y){
            return (x === _x && y === _y);
        }

        /* Update and Display Score */
        /////////////////////////////////////////////////////////////
        let altScore = function(score_val){
            ele_score.innerHTML = String(score_val);
        }

        /* Change Snake Speed */
        /////////////////////////////////////////////////////////////
        let setSnakeSpeed = function(speed_value){
            snake_speed = speed_value;
        }

        /* Wall Collision Logic */
        /////////////////////////////////////////////////////////////
        let setWall = function(wall_value){
            wall = wall_value;
            if(wall === 0){ 
                screen_snake.style.borderColor = "#32691e"; 
            }
            if(wall === 1){ 
                screen_snake.style.borderColor = "#FF0000"; 
            }
        }

        /* Random Food Placement and Logic */
        /////////////////////////////////////////////////////////////
        let addFood = function(){
            food.x = Math.floor(Math.random() * ((canvas.width / BLOCK) - 1));
            food.y = Math.floor(Math.random() * ((canvas.height / BLOCK) - 1));
            for (let i = 0; i < snake.length; i++) {
                if (checkBlock(food.x, food.y, snake[i].x, snake[i].y)) {
                    addFood();  // Reposition food if it overlaps snake
                    return;
                }
            }

            // Generate a random color for the food
            food.color = getRandomColor();
        }

        // Helper function to generate a random color
        let getRandomColor = function(){
            const letters = '0123456789ABCDEF';
            let color = '#';
            for (let i = 0; i < 6; i++){
                color += letters[Math.floor(Math.random() * 16)];
            }
            return color;
        }

        /* Key Input Logic */
        /////////////////////////////////////////////////////////////
        let changeDir = function(key){
            switch(key) {
                case 65: // 'A' Key - Left
                    if (snake_dir !== 1) snake_next_dir = 3;
                    break;
                case 87: // 'W' Key - Up
                    if (snake_dir !== 2) snake_next_dir = 0;
                    break;
                case 68: // 'D' Key - Right
                    if (snake_dir !== 3) snake_next_dir = 1;
                    break;
                case 83: // 'S' Key - Down
                    if (snake_dir !== 0) snake_next_dir = 2;
                    break;
            }
        }

        /* Drawing Snake and Food */
        /////////////////////////////////////////////////////////////
        let activeDot = function(x, y){
            ctx.fillStyle = "#FFFFFF";  // Snake Color
            ctx.fillRect(x * BLOCK, y * BLOCK, BLOCK, BLOCK);
        }

        /* Main Game Loop */
        /////////////////////////////////////////////////////////////
        let mainLoop = function(){
            let _x = snake[0].x;
            let _y = snake[0].y;
            snake_dir = snake_next_dir;

            // Move snake in current direction
            switch(snake_dir){
                case 0: _y--; break;  // Up
                case 1: _x++; break;  // Right
                case 2: _y++; break;  // Down
                case 3: _x--; break;  // Left
            }

            snake.pop();  // Remove tail
            snake.unshift({x: _x, y: _y});  // Add new head position

            // Wall collision logic
            if(wall === 1){
                if (snake[0].x < 0 || snake[0].x >= canvas.width / BLOCK || 
                    snake[0].y < 0 || snake[0].y >= canvas.height / BLOCK){
                    showScreen(SCREEN_GAME_OVER);
                    return;
                }
            } else {
                // Wrap-around logic
                if (snake[0].x < 0) snake[0].x += canvas.width / BLOCK;
                if (snake[0].x >= canvas.width / BLOCK) snake[0].x -= canvas.width / BLOCK;
                if (snake[0].y < 0) snake[0].y += canvas.height / BLOCK;
                if (snake[0].y >= canvas.height / BLOCK) snake[0].y -= canvas.height / BLOCK;
            }

            // Check for snake collision with itself
            for(let i = 1; i < snake.length; i++){
                if (checkBlock(snake[0].x, snake[0].y, snake[i].x, snake[i].y)){
                    showScreen(SCREEN_GAME_OVER);
                    return;
                }
            }

            // Check if snake eats the food
            if(checkBlock(snake[0].x, snake[0].y, food.x, food.y)){
                snake.push({x: snake[0].x, y: snake[0].y});  // Add to snake length
                altScore(++score);  // Increase score
                addFood();  // Reposition food
            }

            // Redraw canvas and snake
            ctx.fillStyle = "black";
            ctx.fillRect(0, 0, canvas.width, canvas.height);
            for(let i = 0; i < snake.length; i++){
                activeDot(snake[i].x, snake[i].y);
            }

            // Draw food
            ctx.fillStyle = food.color;
            activeDot(food.x, food.y);

            setTimeout(mainLoop, snake_speed);
        }

        /* Initialize New Game */
        /////////////////////////////////////////////////////////////
        let newGame = function(){
            showScreen(SCREEN_SNAKE);
            screen_snake.focus();

            score = 0;
            altScore(score);

            snake = [{x: 5, y: 5}];  // Starting position of snake
            snake_next_dir = 1;  // Move right initially
            setSnakeSpeed(120);
            addFood();

            // Add keydown event for snake movement
            canvas.onkeydown = function(evt){
                changeDir(evt.keyCode);
            }
            mainLoop();
        }

        /* Screen Display Logic */
        /////////////////////////////////////////////////////////////
        let showScreen = function(screen_opt){
            SCREEN = screen_opt;
            switch(screen_opt){
                case SCREEN_MENU:
                    screen_menu.style.display = "block";
                    screen_snake.style.display = "none";
                    screen_setting.style.display = "none";
                    screen_game_over.style.display = "none";
                    break;
                case SCREEN_SNAKE:
                    screen_menu.style.display = "none";
                    screen_snake.style.display = "block";
                    screen_setting.style.display = "none";
                    screen_game_over.style.display = "none";
                    break;
                case SCREEN_GAME_OVER:
                    screen_menu.style.display = "none";
                    screen_snake.style.display = "none";
                    screen_setting.style.display = "none";
                    screen_game_over.style.display = "block";
                    break;
                case SCREEN_SETTING:
                    screen_menu.style.display = "none";
                    screen_snake.style.display = "none";
                    screen_setting.style.display = "block";
                    screen_game_over.style.display = "none";
                    break;
            }
        }

        /* Event Listeners and Game Initialization */
        /////////////////////////////////////////////////////////////
        window.onload = function(){
            button_new_game.onclick = newGame;
            button_new_game1.onclick = newGame;
            button_new_game2.onclick = newGame;
            button_setting_menu.onclick = function(){ showScreen(SCREEN_SETTING); };
            button_setting_menu1.onclick = function(){ showScreen(SCREEN_SETTING); };

            window.addEventListener("keydown", function(evt){
                if(evt.code === "Space" && SCREEN !== SCREEN_SNAKE){
                    newGame();
                }
            });
        }

    })();
</script>

___________________________________________________________________________________________________________________________

