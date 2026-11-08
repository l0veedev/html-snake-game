# HTML Snake Game

A lightweight, high-performance Snake game built with pure HTML5 Canvas, CSS, and JavaScript. Written in a single file with zero external dependencies or assets.

### How to Play
- Use the **Arrow Keys** or **W, A, S, D** to control the snake (or swipe on mobile devices).
- Eat the red food to grow and increase your score.
- Going through the walls will teleport you to the opposite side.
- Hitting your own tail resets the game to the starting size.

### Play Online
You can play the game instantly in your browser via [GitHub Pages](https://l0veedev.github.io/html-snake-game/).

---

### Source Code (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HTML Snake Game — Play Online | l0veedev</title>
    <meta name="description" content="A lightweight, high-performance open-source Snake game built with pure HTML5 Canvas and JavaScript. Play online for free, customize, and leave comments!">
    <meta name="keywords" content="html snake game, javascript snake, retro game, indie pet project, l0veedev">
    <meta name="author" content="l0veedev">
    <style>
        body {
            margin: 0;
            padding: 40px 20px;
            background: #0d0d0d;
            display: flex;
            flex-direction: column;
            align-items: center;
            min-height: 100vh;
            box-sizing: border-box;
            outline: none;
        }
        canvas {
            background: #1a1a1a;
            box-shadow: 0 0 20px rgba(0, 0, 0, 0.8);
            border-radius: 4px;
            cursor: pointer;
            margin-bottom: 40px;
            touch-action: none;
        }
        .comments-container {
            width: 100%;
            max-width: 600px;
        }
    </style>
</head>
<body>
    <canvas id="board" width="400" height="400"></canvas>

    <div class="comments-container">
        <script src="[https://giscus.app/client.js](https://giscus.app/client.js)"
                data-repo="l0veedev/html-snake-game"
                data-repo-id="R_kgDOVnXJAA"
                data-category="Announcements"
                data-category-id="DIC_kwDOVnXJAM4DKrfR"
                data-mapping="pathname"
                data-strict="0"
                data-reactions-enabled="1"
                data-emit-metadata="0"
                data-input-position="bottom"
                data-theme="dark"
                data-lang="ru"
                crossorigin="anonymous"
                async>
        </script>
    </div>

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
            if (document.activeElement && document.activeElement.tagName === 'IFRAME') return;

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

        canvas.addEventListener('touchstart', e => {
            touchStartX = e.touches[0].clientX;
            touchStartY = e.touches[0].clientY;
        }, { passive: true });

        canvas.addEventListener('touchend', e => {
            if (!touchStartX || !touchStartY) return;
            let touchEndX = e.changedTouches[0].clientX;
            let touchEndY = e.changedTouches[0].clientY;

            let diffX = touchEndX - touchStartX;
            let diffY = touchEndY - touchStartY;

            if (Math.abs(diffX) > Math.abs(diffY)) {
                if (diffX > 30) changeDirection(size, 0);
                else if (diffX < -30) changeDirection(-size, 0);
            } else {
                if (diffY > 30) changeDirection(0, size);
                else if (diffY < -30) changeDirection(0, -size);
            }
            touchStartX = 0;
            touchStartY = 0;
        }, { passive: true });

        window.addEventListener('keydown', handleInput);
        window.focus();
        initGame();
        gameLoop();
    </script>
</body>
</html>
