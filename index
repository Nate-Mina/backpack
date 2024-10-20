const canvas = document.getElementById('tetris');
const context = canvas.getContext('2d');

// Block size
const blockSize = 32;

// Game board dimensions
const columns = canvas.width / blockSize;
const rows = canvas.height / blockSize;

// Tetromino shapes
const shapes = [
    [ // I
        [0, 1, 0, 0],
        [0, 1, 0, 0],
        [0, 1, 0, 0],
        [0, 1, 0, 0]
    ],
    [ // J
        [0, 1, 0],
        [0, 1, 0],
        [1, 1, 0]
    ],
    [ // L
        [0, 1, 0],
        [0, 1, 0],
        [0, 1, 1]
    ],
    [ // O
        [1, 1],
        [1, 1]
    ],
    [ // S
        [0, 1, 1],
        [1, 1, 0],
        [0, 0, 0]
    ],
    [ // T
        [1, 1, 1],
        [0, 1, 0],
        [0, 0, 0]
    ],
    [ // Z
        [1, 1, 0],
        [0, 1, 1],
        [0, 0, 0]
    ]
];

// Colors for each shape
const colors = [
    'cyan', 'blue', 'orange', 'yellow', 'green', 'purple', 'red'
];

// Create the game board (2D array filled with 0s)
const board = createGameBoard();

// Function to create an empty game board
function createGameBoard() {
    return Array.from({ length: rows }, () => Array(columns).fill(0));
}

// Function to draw a block
function drawBlock(x, y, color) {
    context.fillStyle = color;
    context.fillRect(x * blockSize, y * blockSize, blockSize, blockSize);
    context.strokeStyle = 'black';
    context.strokeRect(x * blockSize, y * blockSize, blockSize, blockSize);
}

// Function to draw the game board
function drawBoard() {
    for (let row = 0; row < rows; row++) {
        for (let col = 0; col < columns; col++) {
            if (board[row][col]) {
                drawBlock(col, row, board[row][col]);
            } else {
                context.fillStyle = 'white';
                context.fillRect(col * blockSize, row * blockSize, blockSize, blockSize);
                context.strokeStyle = 'black';
                context.strokeRect(col * blockSize, row * blockSize, blockSize, blockSize);
            }
        }
    }
}

// Create a new Tetromino
let currentPiece = {
    x: 0,
    y: 0, // Starting position
    shape: shapes[Math.floor(Math.random() * shapes.length)], // Random shape
    color: colors[Math.floor(Math.random() * colors.length)]
};

// Function to draw the current Tetromino
function drawPiece() {
    currentPiece.shape.forEach((row, y) => {
        row.forEach((value, x) => {
            if (value) {
                drawBlock(currentPiece.x + x, currentPiece.y + y, currentPiece.color);
            }
        });
    });
}

// Move the piece down every second
let dropCounter = 0;
let dropInterval = 1000; // 1 second

// Game loop
function update() {
    // Clear the canvas
    context.clearRect(0, 0, canvas.width, canvas.height);

    // Draw the board
    drawBoard();

    // Draw the current piece
    drawPiece();

    // Move the piece down
    dropCounter += 10;
    if (dropCounter > dropInterval) {
        movePieceDown();
        dropCounter = 0;
    }

    requestAnimationFrame(update);
}

// Function to move the piece down
function movePieceDown() {
    // Check for collision below
    if (!collision(0, 1)) {
        currentPiece.y++;
    } else {
        // Freeze the piece and create a new one
        placePiece();
        currentPiece = { ...currentPiece, x: 0, y: 0, shape: shapes[Math.floor(Math.random() * shapes.length)], color: colors[Math.floor(Math.random() * colors.length)] };
    }
}

// Function to check for collisions
function collision(xOffset, yOffset) {
    return currentPiece.shape.some((row, dy) => {
        return row.some((value, dx) => {
            if (!value) return false; // Empty block

            let newX = currentPiece.x + dx + xOffset;
            let newY = currentPiece.y + dy + yOffset;

            // Check if out of bounds or collision with board
            if (newX < 0 || newX >= columns || newY >= rows) return true;
            if (newY < 0) return false; // Ignore collision above the board
            return board[newY][newX] !== 0;
        });
    });
}

// Function to place the piece on the board
function placePiece() {
    currentPiece.shape.forEach((row, y) => {
        row.forEach((value, x) => {
            if (value) {
                board[currentPiece.y + y][currentPiece.x + x] = currentPiece.color;
            }
        });
    });
}

// Event listeners for keyboard controls
document.addEventListener('keydown', (event) => {
    if (event.key === 'ArrowLeft' && !collision(-1, 0)) {
        currentPiece.x--;
    } else if (event.key === 'ArrowRight' && !collision(1, 0)) {
        currentPiece.x++;
    } else if (event.key === 'ArrowDown') {
        movePieceDown();
    }
});

// Start the game loop
update();
