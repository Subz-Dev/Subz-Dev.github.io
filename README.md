<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jogo da Cobrinha</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
        }
        canvas {
            border: 1px solid #000;
        }
    </style>
</head>
<body>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        let snake = [{ x: 10, y: 10 }];
        let direction = { x: 1, y: 0 };
        let food = { x: Math.floor(Math.random() * 20), y: Math.floor(Math.random() * 20) };
        let score = 0;

        function gameLoop() {
            update();
            draw();
            setTimeout(gameLoop, 100);
        }

        function update() {
            const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };
            snake.unshift(head);

            if (head.x === food.x && head.y === food.y) {
                score++;
                food = { x: Math.floor(Math.random() * 20), y: Math.floor(Math.random() * 20) };
            } else {
                snake.pop();
            }

            if (head.x < 0 || head.x >= 20 || head.y < 0 || head.y >= 20 || snake.slice(1).some(segment => segment.x === head.x && segment.y === head.y)) {
                clearInterval(gameLoop);
                alert('Game Over! Score: ' + score);
                document.location.reload();
            }
        }

        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            snake.forEach(segment => {
                ctx.fillStyle = 'green';
                ctx.fillRect(segment.x * 20, segment.y * 20, 18, 18);
            });

            ctx.fillStyle = 'red';
            ctx.fillRect(food.x * 20, food.y * 20, 18, 18);
        }

        document.addEventListener('keydown', (event) => {
            switch (event.key) {
                case 'ArrowUp':
                    if (direction.y === 0) direction = { x: 0, y: -1 };
                    break;
                case 'ArrowDown':
                    if (direction.y === 0) direction = { x: 0, y: 1 };
                    break;
                case 'ArrowLeft':
                    if (direction.x === 0) direction = { x: -1, y: 0 };
                    break;
                case 'ArrowRight':
                    if (direction.x === 0) direction = { x: 1, y: 0 };
                    break;
            }
        });

        gameLoop();
    </script>
</body>
</html>
