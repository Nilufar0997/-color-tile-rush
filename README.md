# -color-tile-rush
 color-tile-rush
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Color Tile Rush</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="game-container">
    <h1>🎨 Color Tile Rush</h1>
    <div id="score">Score: 0</div>
    <div id="target-color">Click on: <span id="color-name">Red</span></div>
    <div id="tile-container"></div>
    <div id="game-over" class="hidden">
      <h2>Game Over!</h2>
      <p>Your Score: <span id="final-score"></span></p>
      <button onclick="startGame()">Play Again</button>
    </div>
  </div>
  <script src="game.js"></script>
</body>
</html>
body {
  margin: 0;
  background: #111;
  font-family: 'Segoe UI', sans-serif;
  color: white;
  text-align: center;
}

.game-container {
  padding: 20px;
}

#tile-container {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  margin-top: 20px;
}

.tile {
  width: 80px;
  height: 80px;
  margin: 10px;
  border-radius: 8px;
  cursor: pointer;
  transition: transform 0.2s;
}

.tile:hover {
  transform: scale(1.1);
}

#game-over.hidden {
  display: none;
}

#color-name {
  font-weight: bold;
}
const colors = ['Red', 'Green', 'Blue', 'Yellow', 'Purple'];
let targetColor = '';
let score = 0;
let speed = 2000;
let tileInterval;
const tileContainer = document.getElementById('tile-container');
const colorName = document.getElementById('color-name');
const scoreDisplay = document.getElementById('score');
const gameOverScreen = document.getElementById('game-over');
const finalScore = document.getElementById('final-score');

function randomColor() {
  return colors[Math.floor(Math.random() * colors.length)];
}

function generateTiles() {
  tileContainer.innerHTML = '';
  for (let i = 0; i < 5; i++) {
    const tile = document.createElement('div');
    const tileColor = randomColor();
    tile.className = 'tile';
    tile.style.backgroundColor = tileColor.toLowerCase();
    tile.dataset.color = tileColor;
    tile.onclick = () => handleClick(tileColor);
    tileContainer.appendChild(tile);
  }
}

function handleClick(clickedColor) {
  if (clickedColor === targetColor) {
    score++;
    scoreDisplay.textContent = `Score: ${score}`;
    updateGameSpeed();
    nextRound();
  } else {
    endGame();
  }
}

function updateGameSpeed() {
  clearInterval(tileInterval);
  speed = Math.max(500, 2000 - score * 100); // faster as score increases
  tileInterval = setInterval(nextRound, speed);
}

function nextRound() {
  targetColor = randomColor();
  colorName.textContent = targetColor;
  generateTiles();
}

function startGame() {
  score = 0;
  speed = 2000;
  scoreDisplay.textContent = `Score: 0`;
  gameOverScreen.classList.add('hidden');
  tileContainer.innerHTML = '';
  nextRound();
  tileInterval = setInterval(nextRound, speed);
}

function endGame() {
  clearInterval(tileInterval);
  finalScore.textContent = score;
  gameOverScreen.classList.remove('hidden');
}

startGame();
