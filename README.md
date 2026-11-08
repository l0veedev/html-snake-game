# HTML Snake Game

A lightweight, high-performance Snake game built with pure HTML5 Canvas, CSS, and JavaScript. Written in a single file with zero external dependencies or assets.

### How to Play
* Use the **Arrow Keys** or **W, A, S, D** to control the snake.
* Eat the red food to grow and increase your score.
* Going through the walls will teleport you to the opposite side.
* Hitting your own tail resets the game to the starting size.

### Play Online
You can play the game instantly in your browser via [GitHub Pages](https://l0veedev.github.io/html-snake-game/).

---

### Source Code (`index.html`)

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Snake Game</title>
    <style>
        body {
            margin: 0;
            background: #0d0d0d;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        canvas {
            background: #1a1a1a;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.8);
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <canvas id="board" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById('board');
        const ctx = canvas.getContext('2d');
        const size = 20;

        let snake, food, dx, dy, lockDirection;

        function initGame() {
            snake = [
                { x: 200, y: 200 },
                { x: 180, y: 200 },
                { x: 160, y: 200 }
            ];
            dx = size;
            dy = 0;
            spawnFood();
        }

        function spawnFood() {
            food = {
                x: Math.floor(Math.random() * (canvas.width / size)) * size,
                y: Math.floor(Math.random() * (canvas.height / size)) * size
            };
            snake.forEach(segment => {
                if (segment.x === food.x && segment.y === food.y) spawnFood();
            });
        }

        function gameLoop() {
            lockDirection = false;
            setTimeout(() => {
                clearScreen();
                drawFood();
                advanceSnake();
                drawSnake();
                gameLoop();
            }, 80);
        }

        function clearScreen() {
            ctx.fillStyle = '#1a1a1a';
            ctx.fillRect(0, 0, canvas.width, canvas.height);
        }

        function drawFood() {
            ctx.fillStyle = '#e53935';
            ctx.fillRect(food.x, food.y, size - 2, size - 2);
        }

        function drawSnake() {
            snake.forEach((part, index) => {
                ctx.fillStyle = index === 0 ? '#4CAF5

<script src="https://giscus.app/client.js"
        data-repo="l0veedev/html-snake-game"
        data-repo-id="R_kgDOVnXJAA"
        data-category="Announcements"
        data-category-id="DIC_kwDOVnXJAM4DKrfR"
        data-mapping="pathname"
        data-strict="0"
        data-reactions-enabled="1"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="preferred_color_scheme"
        data-lang="ru"
        crossorigin="anonymous"
        async>
</script>
