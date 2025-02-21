# snake


_______________________________________________________________
_________________________________________________________________



<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Змейка</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
            background-color: #333;
            font-family: Arial, sans-serif;
        }
        #game-board {
            display: grid;
            grid-template-columns: repeat(20, 20px);
            grid-template-rows: repeat(20, 20px);
            gap: 1px;
            background-color: #222;
            max-width: 420px;
        }
        .cell {
            width: 20px;
            height: 20px;
            background-color: #444;
        }
        .snake {
            background-color: #0f0;
        }
        .food {
            background-color: #f00;
        }
        #score {
            color: #fff;
            font-size: 24px;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div>
        <div id="game-board"></div>
        <div id="score">Счет: 0</div>
    </div>

    <script>
        const boardSize = 20;
        const cellSize = 20;
        const gameBoard = document.getElementById('game-board');
        const scoreElement = document.getElementById('score');
        let snake = [{ x: 10, y: 10 }];
        let food = { x: 5, y: 5 };
        let direction = { x: 0, y: 0 };
        let score = 0;
        let gameInterval;

        function createBoard() {
            for (let y = 0; y < boardSize; y++) {
                for (let x = 0; x < boardSize; x++) {
                    const cell = document.createElement('div');
                    cell.classList.add('cell');
                    gameBoard.appendChild(cell);
                }
            }
        }

        function drawSnake() {
            const cells = document.querySelectorAll('.cell');
            cells.forEach(cell => cell.classList.remove('snake'));

            snake.forEach(segment => {
                const index = segment.y * boardSize + segment.x;
                cells[index].classList.add('snake');
            });
        }

        function drawFood() {
            const cells = document.querySelectorAll('.cell');
            cells.forEach(cell => cell.classList.remove('food'));

            const index = food.y * boardSize + food.x;
            cells[index].classList.add('food');
        }

        function update() {
            const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };

            if (head.x < 0 || head.x >= boardSize || head.y < 0 || head.y >= boardSize || snake.some(segment => segment.x === head.x && segment.y === head.y)) {
                clearInterval(gameInterval);
                alert('Игра окончена! Счет: ' + score);
                return;
            }

            snake.unshift(head);

            if (head.x === food.x && head.y === food.y) {
                score += 10;
                scoreElement.textContent = 'Счет: ' + score;
                placeFood();
            } else {
                snake.pop();
            }

            drawSnake();
            drawFood();
        }

        function placeFood() {
            food = {
                x: Math.floor(Math.random() * boardSize),
                y: Math.floor(Math.random() * boardSize)
            };

            if (snake.some(segment => segment.x === food.x && segment.y === food.y)) {
                placeFood();
            }
        }

        function handleKeyPress(event) {
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
        }

        createBoard();
        placeFood();
        drawSnake();
        drawFood();
        gameInterval = setInterval(update, 100);
        document.addEventListener('keydown', handleKeyPress);
    </script>
</body>
</html>
