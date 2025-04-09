# index.html <html> <head> <title>2-Player Tic Tac Toe</title> <style> table { font-size: 40px; font-family: 'Courier New', Courier, monospace; } </style> </head> <body> <table id="board"></table> <p id="currentPlayer">Current Player: X</p> <button onclick="init()">New Game</button> <script src="tictactoe.js"></script> </body> </html> # tictactoe.js const PLAYER_X = "X"; const PLAYER_O = "O"; const board = document.getElementById("board"); let playerName = PLAYER_X; function makeColumnHtml(id, marker) { return `<td id="${id}" onclick="cellClick(this)">[${marker}]</td>`; } function cellClick(cell) { cell.innerHTML = makeColumnHtml(cell.id, playerName); switchPlayer(); } function displayPlayerName() { document.getElementById("currentPlayer").innerText = "Current Player: " + playerName; } function switchPlayer() { playerName = (playerName == PLAYER_X) ? PLAYER_O : PLAYER_X; displayPlayerName(); } function init() { playerName = PLAYER_X; displayPlayerName(); let boardHtml = ""; for (let ix = 0; ix < 3; ix++) { boardHtml += "<tr>"; for (let jx = 0; jx < 3; jx++) { boardHtml += makeColumnHtml(`${ix}_${jx}`, "_"); } boardHtml += "</tr>"; } board.innerHTML = boardHtml; } init();const PLAYER_X = "X";
const PLAYER_O = "O";
const board = document.getElementById("board");
let playerName = PLAYER_X;
let gameActive = true;
let gameBoard = [
    ["", "", ""],
    ["", "", ""],
    ["", "", ""]
];

function makeColumnHtml(id, marker) {
    return `<td id="${id}" onclick="cellClick(this)">[${marker}]</td>`;
}

function cellClick(cell) {
    if (!gameActive) return; // Block clicks if game is over

    const [row, col] = cell.id.split("_").map(Number);
    if (gameBoard[row][col] !== "") {
        alert("This square is already taken! Choose another one.");
        return;
    }

    gameBoard[row][col] = playerName;
    cell.innerHTML = makeColumnHtml(cell.id, playerName);

    if (checkWinner()) {
        alert(`Player ${playerName} wins!`);
        gameActive = false; // Disable further clicks
        return;
    }

    if (checkTie()) {
        alert("It's a tie!");
        gameActive = false; // Disable further clicks
        return;
    }

    switchPlayer();
}

function checkWinner() {
    // Check rows and columns
    for (let i = 0; i < 3; i++) {
        if (
            gameBoard[i][0] === playerName &&
            gameBoard[i][1] === playerName &&
            gameBoard[i][2] === playerName
        ) return true;
        if (
            gameBoard[0][i] === playerName &&
            gameBoard[1][i] === playerName &&
            gameBoard[2][i] === playerName
        ) return true;
    }

    // Check diagonals
    if (
        gameBoard[0][0] === playerName &&
        gameBoard[1][1] === playerName &&
        gameBoard[2][2] === playerName
    ) return true;
    if (
        gameBoard[0][2] === playerName &&
        gameBoard[1][1] === playerName &&
        gameBoard[2][0] === playerName
    ) return true;

    return false;
}

function checkTie() {
    return gameBoard.flat().every(cell => cell !== "");
}

function displayPlayerName() {
    document.getElementById("currentPlayer").innerText = "Current Player: " + playerName;
}

function switchPlayer() {
    playerName = playerName === PLAYER_X ? PLAYER_O : PLAYER_X;
    displayPlayerName();
}

function init() {
    playerName = PLAYER_X;
    gameActive = true;
    gameBoard = [
        ["", "", ""],
        ["", "", ""],
        ["", "", ""]
    ];
    displayPlayerName();
    let boardHtml = "";
    
    for (let ix = 0; ix < 3; ix++) {
        boardHtml += "<tr>";
        for (let jx = 0; jx < 3; jx++) {
            boardHtml += makeColumnHtml(`${ix}_${jx}`, "_");
        }
        boardHtml += "</tr>";
    }
    
    board.innerHTML = boardHtml;
}

init();

