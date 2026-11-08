# HTML Snake Game

A lightweight, high-performance Snake game built with pure HTML5 Canvas, CSS, and JavaScript. Written in a single file with zero external dependencies or assets.

### How to Play
* Use the **Arrow Keys** or **W, A, S, D** to control the snake (or swipe on mobile devices).
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
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            margin: 0;
            background: #0d0d0d;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            outline: none;
            touch-action: none;
        }
        canvas {
            background: #1a1a1a;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.8);
            border-radius: 4px;
            cursor: pointer;
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
                ctx.fillStyle = index === 0 ? '#4CAF50' : '#81C784';
                ctx.fillRect(part.x, part.y, size - 2, size - 2);
            });
        }

        function advanceSnake() {
            const head = { x: snake[0].x + dx, y: snake[0].y + dy };

            if (head.x < 0) head.x = canvas.width - size;
            else if (head.x >= canvas.width) head.x = 0;

            if (head.y < 0) head.y = canvas.height - size;
            else if (head.y >= canvas.height) head.y = 0;

            for (let i = 1; i < snake.length; i++) {
                if (head.x === snake[i].x && head.y === snake[i].y) {
                    initGame();
                    return;
                }
            }

            snake.unshift(head);

            if (head.x === food.x && head.y === food.y) {
                spawnFood();
            } else {
                snake.pop();
            }
        }

        function changeDirection(newDx, newDy) {
            if (lockDirection) return;
            if (newDx === -dx && newDy === -dy) return;
            
            if (newDx !== 0 && dx === 0) {
                dx = newDx;
                dy = 0;
                lockDirection = true;
            }
            if (newDy !== 0 && dy === 0) {
                dx = 0;
                dy = newDy;
                lockDirection = true;
            }
        }

        function handleInput(event) {
            const key = event.key.toLowerCase();
            if (['arrowup', 'arrowdown', 'arrowleft', 'arrowright', 'w', 'a', 's', 'd'].includes(key)) {
                event.preventDefault();
            }

            if (key === 'arrowleft' || key === 'a') changeDirection(-size, 0);
            if (key === 'arrowup' || key === 'w') changeDirection(0, -size);
            if (key === 'arrowright' || key === 'd') changeDirection(size, 0);
            if (key === 'arrowdown' || key === 's') changeDirection(0, size);
        }

        let touchStartX = 0;
        let touchStartY = 0;
