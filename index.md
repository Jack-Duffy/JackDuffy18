---
layout: base
title: Student Home
description: Home Page
hide: true
---

Who am I?

My Name is jack Duffy and some of my favorite things to do are snowboarding and playing video games. I hope to become an engineer and eventually a buisness owner. Some of my goals in this class are yo learn how to code so I can put my 3d animation skill together with coding and create video games.


<!DOCTYPE html>
<html lang="en">
<head>
    <title>Change Text Size with JavaScript</title>
    <style>
        #text {
            font-size: 16px; /* Default font size */
        }
    </style>
</head>
<body>
    <p id="text">This is some text. You can change its size!</p>
    <button onclick="increaseFontSize()">Increase Size</button>
    <button onclick="decreaseFontSize()">Decrease Size</button>

    <script>
        function increaseFontSize() {
            let textElement = document.getElementById("text");
            let currentSize = parseInt(window.getComputedStyle(textElement).fontSize);
            textElement.style.fontSize = (currentSize + 2) + "px";
        }

        function decreaseFontSize() {
            let textElement = document.getElementById("text");
            let currentSize = parseInt(window.getComputedStyle(textElement).fontSize);
            textElement.style.fontSize = (currentSize - 2) + "px";
        }
    </script>
</body>
</html>
