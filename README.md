# KESHAV0734
# 🌟 PROJECT-X

## 📌 Description
A simple eye-catching arcade game

## 🎨 Demo Preview (HTML & CSS)
Here is a simple **HTML & CSS** snippet from the project:
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Minesweeper Game</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      margin: 0;
      padding: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background-color: #f4f4f4;
    }
    .game-board {
      display: grid;
      grid-template-columns: repeat(10, 30px);
      grid-template-rows: repeat(10, 30px);
      gap: 2px;
      margin: 20px;
      background-color: #bbb;
      border-radius: 10px;
    }
    .cell {
      width: 30px;
      height: 30px;
      background-color: #ddd;
      text-align: center;
      vertical-align: middle;
      font-size: 14px;
      cursor: pointer;
      border-radius: 5px;
      transition: background-color 0.3s;
    }
    .cell.revealed {
      background-color: #e5e5e5;
    }
    .cell.flagged {
      background-color: #ffc107;
    }
    .cell:hover {
      background-color: #bbb;
    }
    .message {
      text-align: center;
      font-size: 24px;
      margin-top: 20px;
      color: #333;
    }
    .cell.bomb {
      background-image: url('https://img.icons8.com/ios/452/bomb.png');
      background-size: cover;
      background-position: center;
    }
  </style>
</head>
<body>

<div class="game-board" id="game-board"></div>
<div class="message" id="game-message"></div>

<script>
  const boardSize = 10;
  const bombCount = 20;
  const board = document.getElementById('game-board');
  const message = document.getElementById('game-message');

  let gameOver = false;
  let flagsLeft = bombCount;
  let cells = [];

  // Function to generate a random number in a range
  function getRandomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
  }

  // Create the board and initialize cells
  function createBoard() {
    cells = [];
    board.innerHTML = '';
    for (let row = 0; row < boardSize; row++) {
      for (let col = 0; col < boardSize; col++) {
        const cell = {
          element: document.createElement('div'),
          row: row,
          col: col,
          isRevealed: false,
          isBomb: false,
          isFlagged: false,
          adjacentBombs: 0
        };
        cell.element.classList.add('cell');
        cell.element.addEventListener('click', () => revealCell(cell));
        cell.element.addEventListener('contextmenu', (e) => {
          e.preventDefault();
          flagCell(cell);
        });
        board.appendChild(cell.element);
        cells.push(cell);
      }
    }
    placeBombs();
    calculateAdjacentBombs();
  }

  // Place bombs randomly on the board
  function placeBombs() {
    let bombsPlaced = 0;
    while (bombsPlaced < bombCount) {
      const index = getRandomInt(0, cells.length - 1);
      const cell = cells[index];
      if (!cell.isBomb) {
        cell.isBomb = true;
        bombsPlaced++;
      }
    }
  }

  // Calculate the number of adjacent bombs for each cell
  function calculateAdjacentBombs() {
    cells.forEach(cell => {
      if (cell.isBomb) return;
      let adjacentBombs = 0;
      for (let rowOffset = -1; rowOffset <= 1; rowOffset++) {
        for (let colOffset = -1; colOffset <= 1; colOffset++) {
          const adjacentCell = getCell(cell.row + rowOffset, cell.col + colOffset);
          if (adjacentCell && adjacentCell.isBomb) {
            adjacentBombs++;
          }
        }
      }
      cell.adjacentBombs = adjacentBombs;
    });
  }

  // Get cell at a specific position
  function getCell(row, col) {
    if (row < 0 || row >= boardSize || col < 0 || col >= boardSize) {
      return null;
    }
    return cells[row * boardSize + col];
  }

  // Reveal a cell when clicked
  function revealCell(cell) {
    if (gameOver || cell.isRevealed || cell.isFlagged) return;

    cell.isRevealed = true;
    cell.element.classList.add('revealed');
    if (cell.isBomb) {
      cell.element.classList.add('bomb');
      gameOver = true;
      message.textContent = 'Game Over! You hit a bomb!';
      revealAllBombs();
    } else {
      cell.element.textContent = cell.adjacentBombs || '';
      if (cell.adjacentBombs === 0) {
        revealAdjacentCells(cell);
      }
    }

    if (checkWin()) {
      gameOver = true;
      message.textContent = 'Congratulations! You won!';
    }
  }

  // Flag a cell as a potential bomb location
  function flagCell(cell) {
    if (gameOver || cell.isRevealed) return;

    if (cell.isFlagged) {
      cell.isFlagged = false;
      flagsLeft++;
      cell.element.classList.remove('flagged');
    } else if (flagsLeft > 0) {
      cell.isFlagged = true;
      flagsLeft--;
      cell.element.classList.add('flagged');
    }
    updateFlagCount();
  }

  // Update the flag count display
  function updateFlagCount() {
    message.textContent = `Flags left: ${flagsLeft}`;
  }

  // Reveal all adjacent cells if they have no bombs around
  function revealAdjacentCells(cell) {
    for (let rowOffset = -1; rowOffset <= 1; rowOffset++) {
      for (let colOffset = -1; colOffset <= 1; colOffset++) {
        const adjacentCell = getCell(cell.row + rowOffset, cell.col + colOffset);
        if (adjacentCell && !adjacentCell.isRevealed && !adjacentCell.isBomb) {
          revealCell(adjacentCell);
        }
      }
    }
  }

  // Reveal all bombs (when the game is over)
  function revealAllBombs() {
    cells.forEach(cell => {
      if (cell.isBomb && !cell.isRevealed) {
        cell.element.classList.add('bomb');
      }
    });
  }

  // Check if the player has won
  function checkWin() {
    return cells.every(cell => {
      if (cell.isBomb) return cell.isFlagged;
      return cell.isRevealed;
    });
  }

  // Initialize the game
  createBoard();
  updateFlagCount();
</script>

</body>
</html>
📌 Output Preview: nice bomb and mine game
________________________________________
🔹 Features
•	🖼️ Beautiful UI with simple HTML & CSS.
•	🚀 Fast and efficent
•	🛠️ 24X7 customer support
🚀 How to Run the Project
1.	Clone the repository: 
bash
CopyEdit
git clone https://github.com/dhimanz/KESHAV0734.git
2.	Navigate to the project directory: 
bash
CopyEdit
cd KESHAV0734
3.	Open index.html in a browser to see the output.
________________________________________
🤝 Contribution Guidelines
1.	Fork the repository.
2.	Create a new branch for your changes: 
bash
CopyEdit
git checkout -b feature-branch
3.	Make changes and commit: 
bash
CopyEdit
git commit -m "Added new feature"
4.	Push to GitHub and create a Pull Request.
________________________________________
📜 License
This project is licensed under MIT License.
👥 Team & Contributors
•	Your Name
•	Contributor Name
________________________________________
🎯 Task 1 Deliverables
✅ A completed README.md file in the GitHub repository with formatted text and HTML & CSS snippets.
✅ Preview of HTML & CSS Code inside the README.
✅ Project features and how-to instructions included.
________________________________________
📌 Part 2: Adding Issues on GitHub
Step 1: Navigate to the Issues Section
1.	Go to your GitHub repository.
2.	Click on the "Issues" tab.
Step 2: Create New Issues
1.	Click "New issue".
2.	Fill in the details: 
o	Title: Clearly describe the issue (e.g., "Improve button styling in CSS").
o	Description: Provide a detailed explanation.
o	Labels: Assign a label (e.g., "bug", "enhancement", "documentation").
3.	Click "Submit new issue".
Step 3: Assign Issues
1.	Open an issue.
2.	Click "Assignees" and assign it to yourself or a team member.
3.	Click "Milestones" (optional) to track project goals.
________________________________________
🎯 Task 2 Deliverables
✅ At least 3 GitHub issues created for project tracking.
✅ Each issue should have a clear title and description.
✅ Issues should be assigned and categorized using labels.
________________________________________
📌 Bonus Challenge 🎯
🔹 Add a screenshot of your HTML output in the README.
🔹 Use Markdown styling (bold, italic, headers) to make the README attractive.
🔹 Customize the CSS styles and improve the UI.



