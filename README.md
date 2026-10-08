# yilan-oyunu<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Yılan Oyunu</title>
  <style>
    body {
      background-color: #121212;
      color: white;
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      margin: 0;
      height: 100vh;
      touch-action: manipulation;
    }
    #score { font-size: 20px; margin-bottom: 10px; }
    canvas {
      background-color: #000;
      border: 2px solid #fff;
      box-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
    }
    .controls {
      display: grid;
      grid-template-columns: repeat(3, 60px);
      grid-gap: 10px;
      margin-top: 15px;
    }
    .btn {
      width: 60px;
      height: 60px;
      background-color: #333;
      color: white;
      font-size: 20px;
      border: none;
      border-radius: 10px;
      display: flex;
      align-items: center;
      justify-content: center;
      user-select: none;
    }
    .btn:active { background-color: #555; }
    .empty { visibility: hidden; }
  </style>
</head>
<body>

<div id="score">Skor: 0</div>
<canvas id="game" width="300" height="300"></canvas>

<div class="controls">
  <div class="empty"></div>
  <button class="btn" onclick="changeDir('UP')">▲</button>
  <div class="empty"></div>
  <button class="btn" onclick="changeDir('LEFT')">◄</button>
  <button class="btn" onclick="changeDir('DOWN')">▼</button>
  <button class="btn" onclick="changeDir('RIGHT')">►</button>
</div>

<script>
  const canvas = document.getElementById("game");
  const ctx = canvas.getContext("2d");
  const box = 15;
  let score = 0;

  let snake = [{ x: 10 * box, y: 10 * box }];
  let food = {
    x: Math.floor(Math.random() * 19 + 1) * box,
    y: Math.floor(Math.random() * 19 + 1) * box
  };
  let d = null;

  function changeDir(dir) {
    if (dir === 'LEFT' && d !== 'RIGHT') d = 'LEFT';
    if (dir === 'UP' && d !== 'DOWN') d = 'UP';
    if (dir === 'RIGHT' && d !== 'LEFT') d = 'RIGHT';
    if (dir === 'DOWN' && d !== 'UP') d = 'DOWN';
  }

  function draw() {
    ctx.fillStyle = "black";
    ctx.fillRect(0, 0, canvas.width, canvas.height);

    for (let i = 0; i < snake.length; i++) {
      ctx.fillStyle = (i === 0) ? "green" : "white";
      ctx.fillRect(snake[i].x, snake[i].y, box, box);
    }

    ctx.fillStyle = "red";
    ctx.fillRect(food.x, food.y, box, box);

    let snakeX = snake[0].x;
    let snakeY = snake[0].y;

    if (d === "LEFT") snakeX -= box;
    if (d === "UP") snakeY -= box;
    if (d === "RIGHT") snakeX += box;
    if (d === "DOWN") snakeY += box;

    if (snakeX === food.x && snakeY === food.y) {
      score++;
      document.getElementById("score").innerText = "Skor: " + score;
      food = {
        x: Math.floor(Math.random() * 19 + 1) * box,
        y: Math.floor(Math.random() * 19 + 1) * box
      };
    } else {
      snake.pop();
    }

    let newHead = { x: snakeX, y: snakeY };

    if (
      snakeX < 0 || snakeX >= canvas.width ||
      snakeY < 0 || snakeY >= canvas.height ||
      collision(newHead, snake)
    ) {iPhone cihazlarda doğrudan bir `.exe` ya da standart kod çalıştırmak iOS'un güvenlik kısıtlamaları nedeniyle mümkün değildir. Ancak **GitHub Pages** ve **PWA (Progressive Web App)** teknolojisini kullanarak tam ekran çalışan, tıpkı bir uygulama gibi ana ekrana ekleyebileceğiniz dokunmatik kontrollü bir Yılan Oyunu oluşturabilirsiniz.

Aşağıdaki adımları sırasıyla takip edebilirsiniz:

<Sequence>
  <Step title="GitHub Deposu (Repository) Oluşturun" subtitle="3 dakika">
    1. **github.com** adresine girip hesabınıza giriş yapın.
    2. Sağ üstteki **"+"** ikonuna tıklayıp **New repository** seçeneğini seçin.
    3. **Repository name** kısmına `yilan-oyunu` yazın.
    4. Gizlilik ayarını **Public** yapın.
    5. **Add a README file** seçeneğini işaretleyip en alttaki **Create repository** butonuna basın.
  </Step>

  <Step title="Oyun Kodlarını Ekleyin" subtitle="5 dakika">
    1. Oluşturduğunuz depoda **Add file** > **Create new file** butonuna tıklayın.
    2. Dosya adını **`index.html`** yapın.
    3. Aşağıdaki kod bloğunun tamamını kopyalayıp editör alanına yapıştırın:

```html
<!DOCTYPE html>
<html lang="tr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
  <title>Yılan Oyunu</title>
  <style>
    * { touch-action: manipulation; }
    body {
      margin: 0;
      background-color: #121212;
      color: #fff;
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      min-height: 100vh;
    }
    #score { font-size: 24px; margin-bottom: 10px; font-weight: bold; }
    canvas {
      border: 2px solid #333;
      background-color: #000;
      box-shadow: 0 4px 10px rgba(0,0,0,0.5);
      border-radius: 8px;
    }
    .controls {
      display: grid;
      grid-template-columns: repeat(3, 70px);
      grid-template-rows: repeat(3, 70px);
      gap: 10px;
      margin-top: 20px;
    }
    .btn {
      background-color: #333;
      color: #fff;
      border: none;
      border-radius: 12px;
      font-size: 24px;
      display: flex;
      align-items: center;
      justify-content: center;
      user-select: none;
      -webkit-user-select: none;
    }
    .btn:active { background-color: #555; }
    .btn-up { grid-column: 2; grid-row: 1; }
    .btn-left { grid-column: 1; grid-row: 2; }
    .btn-down { grid-column: 2; grid-row: 2; }
    .btn-right { grid-column: 3; grid-row: 2; }
  </style>
</head>
<body>

  <div id="score">Skor: 0</div>
  <canvas id="gameCanvas" width="300" height="300"></canvas>

  <div class="controls">
    <button class="btn btn-up" onclick="changeDir('UP')">▲</button>
    <button class="btn btn-left" onclick="changeDir('LEFT')">◀</button>
    <button class="btn btn-down" onclick="changeDir('DOWN')">▼</button>
    <button class="btn btn-right" onclick="changeDir('RIGHT')">▶</button>
  </div>

  <script>
    const canvas = document.getElementById("gameCanvas");
    const ctx = canvas.getContext("2d");
    const grid = 15;
    let score = 0;
    
    let snake = [{x: 150, y: 150}];
    let food = {x: 60, y: 60};
    let dx = grid;
    let dy = 0;

    function gameLoop() {
      setTimeout(() => {
        clearCanvas();
        drawFood();
        moveSnake();
        drawSnake();
        if (!checkGameOver()) {
          gameLoop();
        } else {
          alert("Oyun Bitti! Skorunuz: " + score);
          resetGame();
        }
      }, 120);
    }

    function clearCanvas() {
      ctx.fillStyle = "#000";
      ctx.fillRect(0, 0, canvas.width, canvas.height);
    }

    function drawSnake() {
      ctx.fillStyle = "#4CAF50";
      snake.forEach((part, index) => {
        if (index === 0) ctx.fillStyle = "#81C784";
        else ctx.fillStyle = "#4CAF50";
        ctx.fillRect(part.x, part.y, grid - 1, grid - 1);
      });
    }

    function moveSnake() {
      const head = {x: snake[0].x + dx, y: snake[0].y + dy};
      snake.unshift(head);

      if (head.x === food.x && head.y === food.y) {
        score += 10;
        document.getElementById("score").innerText = "Skor: " + score;
        generateFood();
      } else {
        snake.pop();
      }
    }

    function generateFood() {
      food.x = Math.floor(Math.random() * (canvas.width / grid)) * grid;
      food.y = Math.floor(Math.random() * (canvas.height / grid)) * grid;
    }

    function drawFood() {
      ctx.fillStyle = "#FF5252";
      ctx.fillRect(food.x, food.y, grid - 1, grid - 1);
    }

    function changeDir(direction) {
      if (direction === 'LEFT' && dx === 0) { dx = -grid; dy = 0; }
      if (direction === 'UP' && dy === 0) { dx = 0; dy = -grid; }
      if (direction === 'RIGHT' && dx === 0) { dx = grid; dy = 0; }
      if (direction === 'DOWN' && dy === 0) { dx = 0; dy = grid; }
    }

    function checkGameOver() {
      const head = snake[0];
      if (head.x < 0 || head.x >= canvas.width || head.y < 0 || head.y >= canvas.height) return true;
      for (let i = 1; i < snake.length; i++) {
        if (head.x === snake[i].x && head.y === snake[i].y) return true;
      }
      return false;
    }

    function resetGame() {
      snake = [{x: 150, y: 150}];
      dx = grid;
      dy = 0;
      score = 0;
      document.getElementById("score").innerText = "Skor: " + score;
      generateFood();
      gameLoop();
    }

    generateFood();
    gameLoop();
  </script>
</body>
</html>
