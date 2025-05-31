#html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Tic Tac Toe</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>
  <div class="game-container">
    <h1>Tic Tac Toe</h1>
    <div class="board" id="board">
      <!-- 3x3 Grid -->
      <div class="cell" data-index="0"></div>
      <div class="cell" data-index="1"></div>
      <div class="cell" data-index="2"></div>
      <div class="cell" data-index="3"></div>
      <div class="cell" data-index="4"></div>
      <div class="cell" data-index="5"></div>
      <div class="cell" data-index="6"></div>
      <div class="cell" data-index="7"></div>
      <div class="cell" data-index="8"></div>
    </div>
    <p id="status">Player X's turn</p>
    <button onclick="resetGame()">Reset</button>
  </div>
  <script src="script.js"></script>
</body>
</html>

#css
body {
  background-color: #1e1e1e;
  color: white;
  font-family: Arial, sans-serif;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.game-container {
  text-align: center;
}

.board {
  display: grid;
  grid-template-columns: repeat(3, 100px);
  gap: 10px;
  margin: 20px auto;
}

.cell {
  width: 100px;
  height: 100px;
  background-color: #444;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 2.5em;
  cursor: pointer;
  border-radius: 10px;
  user-select: none;
  transition: background-color 0.2s;
}

.cell:hover {
  background-color: #666;
}

button {
  padding: 10px 20px;
  background-color: #5c67f2;
  color: white;
  border: none;
  border-radius: 8px;
  font-size: 1em;
  cursor: pointer;
  margin-top: 10px;
}

button:hover {
  background-color: #7a85f7;
}

#js
let currentPlayer = "X";
let board = ["", "", "", "", "", "", "", "", ""];
let gameActive = true;
const statusDisplay = document.getElementById("status");
const cells = document.querySelectorAll(".cell");

const winningCombinations = [
  [0,1,2], [3,4,5], [6,7,8], // Rows
  [0,3,6], [1,4,7], [2,5,8], // Columns
  [0,4,8], [2,4,6]           // Diagonals
];

cells.forEach(cell => {
  cell.addEventListener("click", () => {
    const index = cell.dataset.index;
    if (board[index] === "" && gameActive) {
      board[index] = currentPlayer;
      cell.textContent = currentPlayer;
      checkResult();
      currentPlayer = currentPlayer === "X" ? "O" : "X";
      if (gameActive) statusDisplay.textContent = `Player ${currentPlayer}'s turn`;
    }
  });
});

function checkResult() {
  for (let combo of winningCombinations) {
    const [a, b, c] = combo;
    if (board[a] && board[a] === board[b] && board[a] === board[c]) {
      statusDisplay.textContent = `🎉 Player ${board[a]} wins!`;
      gameActive = false;
      return;
    }
  }

  if (!board.includes("")) {
    statusDisplay.textContent = "It's a draw!";
    gameActive = false;
  }
}

function resetGame() {
  board = ["", "", "", "", "", "", "", "", ""];
  currentPlayer = "X";
  gameActive = true;
  statusDisplay.textContent = "Player X's turn";
  cells.forEach(cell => cell.textContent = "");
}
