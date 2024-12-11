---
layout: base
title: Student Home
description: Home Page
hide: true
---
-
# About Me 
Not in About(Dont worry about it)


___________________________________________________________________________________________________________________________

<img src="images/IMG_5269 copy.JPG" alt="Description"
style="width:400px; height:auto;">

___________________________________________________________________________________________________________________________

This is my Dog Named Piper, She is 8 years old and is a labradoodle.

___________________________________________________________________________________________________________________________

<img src="images/Mountain.webp" alt="Description"
style="width:400px; height:auto;">

___________________________________________________________________________________________________________________________

One of my favorite things to do is snowboarding in the winter. That is why it is my favorite time of year, my favorite resorts are <i>Mammoth Mountain</i> and <i>Brian Head</i> in utah.

 __________________________________________________________________________________________________________________________

<img src="images/x2.jpeg" alt="Description"
style="width:400px; height:auto;">

___________________________________________________________________________________________________________________________

Another one of my favorite things is riding roller coasters my favorite roller coaster right now is X2 at <i>Six Flags Magic Mountain</i>.

___________________________________________________________________________________________________________________________


<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dinosaur Jump Game</title>
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
        #game {
            position: relative;
            width: 600px;
            height: 200px;
            background-color: #fff;
            border: 2px solid #000;
            overflow: hidden;
        }
        #dino {
            position: absolute;
            bottom: 20px;
            left: 50px;
            width: 40px;
            height: 40px;
            background-color: green;
            border-radius: 5px;
        }
        #cactus {
            position: absolute;
            bottom: 20px;
            right: 0;
            width: 20px;
            height: 40px;
            background-color: red;
        }
    </style>
</head>
<body>
    <div id="game">
        <div id="dino"></div>
        <div id="cactus"></div>
    </div>
    <script>
        const dino = document.getElementById("dino");
        const cactus = document.getElementById("cactus");
        let isJumping = false;

        function jump() {
            if (isJumping) return;
            isJumping = true;
            let upInterval = setInterval(() => {
                let dinoBottom = parseInt(window.getComputedStyle(dino).getPropertyValue("bottom"));
                if (dinoBottom >= 150) {
                    clearInterval(upInterval);
                    let downInterval = setInterval(() => {
                        if (dinoBottom <= 20) {
                            clearInterval(downInterval);
                            isJumping = false;
                        }
                        dinoBottom -= 5;
                        dino.style.bottom = dinoBottom + "px";
                    }, 20);
                }
                dinoBottom += 5;
                dino.style.bottom = dinoBottom + "px";
            }, 20);
        }

        document.addEventListener("keydown", (event) => {
            if (event.key === " " || event.key === "ArrowUp") {
                jump();
            }
        });

        function moveCactus() {
            let cactusLeft = parseInt(window.getComputedStyle(cactus).getPropertyValue("left"));
            if (cactusLeft < -20) {
                cactus.style.left = "600px";
            } else {
                cactus.style.left = cactusLeft - 5 + "px";
            }
        }

        setInterval(moveCactus, 20);
    </script>
</body>
</html>
