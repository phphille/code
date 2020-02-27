[
        [
          [
            [0, 0],
            [0, 0],
            [0, 0],
            [0, 0]
          ],
          [
            [0, 0],
            [0, 0],
            [0, 0],
            [0, 0]
          ],
          [
            [0, 0],
            [0, 0],
            [0, 0],
            [0, 0]
          ],
          [
            [0, 0],
            [0, 0],
            [0, 0],
            [0, 0]
          ]
        ],
        [
          [
            [0, 0],
            [1, 0],
            [0, 1],
            [1, 1]
          ],
          [
            [0, 0],
            [1, 0],
            [0, 1],
            [1, 1]
          ],
          [
            [0, 0],
            [1, 0],
            [0, 1],
            [1, 1]
          ],
          [
            [0, 0],
            [1, 0],
            [0, 1],
            [1, 1]
          ]
        ],
        [
          [
            [0, -1],
            [0, 0],
            [0, 1],
            [0, 2]
          ],
          [
            [-1, 0],
            [0, 0],
            [1, 0],
            [2, 0]
          ],
          [
            [0, -1],
            [0, 0],
            [0, 1],
            [0, 2]
          ],
          [
            [-1, 0],
            [0, 0],
            [1, 0],
            [2, 0]
          ]
        ],
        [
          [
            [0, 0],
            [-1, 0],
            [1, 0],
            [0, -1]
          ],
          [
            [0, 0],
            [1, 0],
            [0, 1],
            [0, -1]
          ],
          [
            [0, 0],
            [-1, 0],
            [1, 0],
            [0, 1]
          ],
          [
            [0, 0],
            [-1, 0],
            [0, 1],
            [0, -1]
          ]
        ],
        [
          [
            [0, 0],
            [-1, 0],
            [1, 0],
            [-1, -1]
          ],
          [
            [0, 0],
            [0, 1],
            [0, -1],
            [1, -1]
          ],
          [
            [0, 0],
            [1, 0],
            [-1, 0],
            [1, 1]
          ],
          [
            [0, 0],
            [0, 1],
            [0, -1],
            [-1, 1]
          ]
        ],
        [
          [
            [0, 0],
            [1, 0],
            [-1, 0],
            [1, -1]
          ],
          [
            [0, 0],
            [0, 1],
            [0, -1],
            [1, 1]
          ],
          [
            [0, 0],
            [1, 0],
            [-1, 0],
            [-1, 1]
          ],
          [
            [0, 0],
            [0, 1],
            [0, -1],
            [-1, -1]
          ]
        ],
        [
          [
            [0, 0],
            [1, 0],
            [0, -1],
            [-1, -1]
          ],
          [
            [0, 0],
            [1, 0],
            [0, 1],
            [1, -1]
          ],
          [
            [0, 0],
            [1, 0],
            [0, -1],
            [-1, -1]
          ],
          [
            [0, 0],
            [1, 0],
            [0, 1],
            [1, -1]
          ]
        ],
        [
          [
            [0, 0],
            [-1, 0],
            [0, -1],
            [1, -1]
          ],
          [
            [0, 0],
            [0, -1],
            [1, 0],
            [1, 1]
          ],
          [
            [0, 0],
            [-1, 0],
            [0, -1],
            [1, -1]
          ],
          [
            [0, 0],
            [0, -1],
            [1, 0],
            [1, 1]
          ]
        ]
      ]
      
      
      
      
      
      
      
      
      
      
      
      componentDidMount() {
    let timerId;
    timerId = window.setInterval(
      () => this.handleBoardUpdate("down"),
      1000 - (this.state.level * 10 > 600 ? 600 : this.state.level * 10)
    );
    this.setState({
      timerId: timerId
    });
    document.addEventListener("keydown", e =>
      this.handleBoardUpdate(undefined, e)
    );
  }
  /**
   * @description Resets the timer when component unmounts
   * @memberof Tetris
   */
  componentWillUnmount() {
    window.clearInterval(this.state.timerId);
  }
  /**
   * @description Handles board updates
   * @param {string} command
   * @memberof Tetris
   */
  handleBoardUpdate(command?: string, e?: any) {
    if (this.state.gameOver || this.state.isPaused) {
      return;
    }
    let xAdd = 0;
    let yAdd = 0;
    let rotateAdd = 0;
    let tile = this.state.activeTile;
    if (command === "left" || e?.keyCode === 37) {
      xAdd = -1;
    }
    if (command === "right" || e?.keyCode === 39) {
      xAdd = 1;
    }
    if (command === "rotate" || e?.keyCode === 32) {
      rotateAdd = 1;
    }
    if (command === "down" || e?.keyCode === 40) {
      yAdd = 1;
    }
    let field = this.state.field;
    let x = this.state.activeTileX;
    let y = this.state.activeTileY;
    let rotate = this.state.tileRotate;
    const tiles = this.state.tiles;
    field[y + tiles[tile][rotate][0][1]][x + tiles[tile][rotate][0][0]] = 0;
    field[y + tiles[tile][rotate][1][1]][x + tiles[tile][rotate][1][0]] = 0;
    field[y + tiles[tile][rotate][2][1]][x + tiles[tile][rotate][2][0]] = 0;
    field[y + tiles[tile][rotate][3][1]][x + tiles[tile][rotate][3][0]] = 0;
    let xAddIsValid = true;
    if (xAdd !== 0) {
      for (let i = 0; i <= 3; i++) {
        if (
          x + xAdd + tiles[tile][rotate][i][0] >= 0 &&
          x + xAdd + tiles[tile][rotate][i][0] < this.props.boardWidth
        ) {
          if (
            field[y + tiles[tile][rotate][i][1]][
              x + xAdd + tiles[tile][rotate][i][0]
            ] !== 0
          ) {
            xAddIsValid = false;
          }
        } else {
          xAddIsValid = false;
        }
      }
    }
    if (xAddIsValid) {
      x += xAdd;
    }
    let newRotate = rotate + rotateAdd > 3 ? 0 : rotate + rotateAdd;
    let rotateIsValid = true;
    if (rotateAdd !== 0) {
      for (let i = 0; i <= 3; i++) {
        if (
          x + tiles[tile][newRotate][i][0] >= 0 &&
          x + tiles[tile][newRotate][i][0] < this.props.boardWidth &&
          y + tiles[tile][newRotate][i][1] >= 0 &&
          y + tiles[tile][newRotate][i][1] < this.props.boardHeight
        ) {
          if (
            field[y + tiles[tile][newRotate][i][1]][
              x + tiles[tile][newRotate][i][0]
            ] !== 0
          ) {
            rotateIsValid = false;
          }
        } else {
          rotateIsValid = false;
        }
      }
    }
    if (rotateIsValid) {
      rotate = newRotate;
    }
    let yAddIsValid = true;
    if (yAdd !== 0) {
      for (let i = 0; i <= 3; i++) {
        if (
          y + yAdd + tiles[tile][rotate][i][1] >= 0 &&
          y + yAdd + tiles[tile][rotate][i][1] < this.props.boardHeight
        ) {
          if (
            field[y + yAdd + tiles[tile][rotate][i][1]][
              x + tiles[tile][rotate][i][0]
            ] !== 0
          ) {
            // Prevent faster fall
            yAddIsValid = false;
          }
        } else {
          // Prevent faster fall
          yAddIsValid = false;
        }
      }
    }
    // If speeding up the fall is valid (move the tile down faster)
    if (yAddIsValid) {
      y += yAdd;
    }
    // Render the tile at new position
    field[y + tiles[tile][rotate][0][1]][x + tiles[tile][rotate][0][0]] = tile;
    field[y + tiles[tile][rotate][1][1]][x + tiles[tile][rotate][1][0]] = tile;
    field[y + tiles[tile][rotate][2][1]][x + tiles[tile][rotate][2][0]] = tile;
    field[y + tiles[tile][rotate][3][1]][x + tiles[tile][rotate][3][0]] = tile;
    // If moving down is not possible, remove completed rows add score
    // and find next tile and check if game is over
    if (!yAddIsValid) {
      for (let row = this.props.boardHeight - 1; row >= 0; row--) {
        let isLineComplete = true;
        // Check if row is completed
        for (let col = 0; col < this.props.boardWidth; col++) {
          if (field[row][col] === 0) {
            isLineComplete = false;
          }
        }
        // Remove completed rows
        if (isLineComplete) {
          for (let yRowSrc = row; row > 0; row--) {
            for (let col = 0; col < this.props.boardWidth; col++) {
              field[row][col] = field[row - 1][col];
            }
          }
          // Check if the row is the last
          row = this.props.boardHeight;
        }
      }
      // Update state - update score, update number of tiles, change level
      this.setState(prev => ({
        score: prev.score + 1 * prev.level,
        tileCount: prev.tileCount + 1,
        level: 1 + Math.floor(prev.tileCount / 10)
      }));
      // Prepare new timer
      let timerId;
      // Reset the timer
      clearInterval(this.state.timerId);
      // Update new timer
      timerId = setInterval(
        () => this.handleBoardUpdate("down", undefined),
        1000 - (this.state.level * 10 > 600 ? 600 : this.state.level * 10)
      );
      // Use new timer
      this.setState({
        timerId: timerId
      });
      // Create new tile
      tile = Math.floor(Math.random() * 7 + 1);
      x = this.props.boardWidth / 2;
      y = 1;
      rotate = 0;
      // Test if game is over - test if new tile can't be placed in field
      if (
        field[y + tiles[tile][rotate][0][1]][x + tiles[tile][rotate][0][0]] !==
          0 ||
        field[y + tiles[tile][rotate][1][1]][x + tiles[tile][rotate][1][0]] !==
          0 ||
        field[y + tiles[tile][rotate][2][1]][x + tiles[tile][rotate][2][0]] !==
          0 ||
        field[y + tiles[tile][rotate][3][1]][x + tiles[tile][rotate][3][0]] !==
          0
      ) {
        // Stop the game
        this.setState({
          gameOver: true
        });
      } else {
        // Otherwise, render new tile and continue
        field[y + tiles[tile][rotate][0][1]][
          x + tiles[tile][rotate][0][0]
        ] = tile;
        field[y + tiles[tile][rotate][1][1]][
          x + tiles[tile][rotate][1][0]
        ] = tile;
        field[y + tiles[tile][rotate][2][1]][
          x + tiles[tile][rotate][2][0]
        ] = tile;
        field[y + tiles[tile][rotate][3][1]][
          x + tiles[tile][rotate][3][0]
        ] = tile;
      }
    }
    // Update state - use new field, active x/y coordinates, rotation and activeTile
    this.setState({
      field: field,
      activeTileX: x,
      activeTileY: y,
      tileRotate: rotate,
      activeTile: tile
    });
  }
  /**
   * @description Stops and resumes the game
   * @memberof Tetris
   */
  handlePauseClick = () => {
    this.setState(prev => ({
      isPaused: !prev.isPaused
    }));
  };
  /**
   * @description Resets the game
   * @memberof Tetris
   */
  handleNewGameClick = () => {
    let field: any[] = [];
    for (let y = 0; y < this.props.boardHeight; y++) {
      let row = [];
      for (let x = 0; x < this.props.boardWidth; x++) {
        row.push(0);
      }
      field.push(row);
    }
    let xStart = Math.floor(this.props.boardWidth / 2);
    this.setState({
      activeTileX: xStart,
      activeTileY: 1,
      activeTile: 2,
      tileRotate: 0,
      score: 0,
      level: 1,
      tileCount: 0,
      gameOver: false,
      field: field
    });
  };
  
  
  /* Main styles */
html {
  box-sizing: border-box;
  font: 16px sans-serif;
}
*,
*::before,
*::after {
  box-sizing: inherit;
}
@keyframes slideInFromTop {
  0% {
    transform: translateY(-100%);
    opacity: 0;
  }
  100% {
    transform: translateY(0);
    opacity: 1;
  }
}
@keyframes fadeIn {
  0% {
    opacity: 0;
  }
  100% {
    opacity: 1;
  }
}
.container {
  height: auto;
  width: 100vw;
  background-color: #a5b1aa;
  font-family: Arial, Helvetica, sans-serif;
  padding-bottom: 48px;
}
.header {
  position: relative;
  height: 72px;
  background-color: #b7debd;
  width: 100%;
  animation: 1s ease-out 0s 1 slideInFromTop;
}
.brand-logo {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  max-width: 100%;
  height: 120px;
}
.work-area {
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  width: 75%;
  height: auto;
  min-height: calc(100vh - 168px);
  margin-left: auto;
  margin-right: auto;
  margin-top: 48px;
  background-color: #f1f0ed;
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
  animation: 2s ease-out 0s 1 fadeIn;
}
.welcome-container {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  color: #333333;
  animation: 2s ease-out 0s 1 fadeIn;
  width: 100%;
}
.heading {
  color: #a5b1aa;
}
.start {
  background-color: #fac0b1; /* Green */
  border: none;
  color: #f1f0ed;
  padding: 16px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 8px;
  font-weight: 700;
}
.start:focus {
  outline: none;
}
.start:hover {
  opacity: 0.7;
}
.ready-player-one {
  color: #4b6455;
  font-weight: 700;
}
@media screen and (max-width: 1024px) {
  .work-area {
    width: 85%;
    margin-top: 72px;
  }
  .brand-logo {
    height: auto;
  }
}
@media screen and (max-width: 767px) {
  .work-area {
    width: 100%;
    margin-top: 48px;
  }
  .brand-logo {
    height: auto;
  }
}
.tetris {
  padding: 8px;
  margin: 0 auto;
  width: 500px;
}
.tetris-board {
  display: flex;
  justify-content: center;
  position: relative;
}
.tetris-board__info {
  width: 100px;
  position: absolute;
  top: 0;
  left: -100px;
}
.tetris-board__text {
  font-size: 18px;
  color: #111;
}
.tetris-board__row {
  display: flex;
}
/* Styles for tiles */
[class*="col-"] {
  padding: 12px;
  border: 1px solid #1a1c19;
}
/* Default (empty) board column */
.col-0 {
  background-color: #333333;
}
/* Colors for tiles */
.col-1 {
  background-color: #fac0b1;
}
.col-2 {
  background-color: #dbeede;
}
.col-3 {
  background-color: #e4e1db;
}
.col-4 {
  background-color: #fcdfd8;
}
.col-5 {
  background-color: #b7debd;
}
.col-6 {
  background-color: #f1f0ed;
}
.col-7 {
  background-color: #4b6455;
}
/* Styles for buttons */
.tetris__block-controls,
.tetris__game-controls {
  margin-top: 16px;
  display: flex;
  justify-content: center;
}
.tetris__game-controls {
  margin-bottom: 16px;
}
.btn {
  padding: 12px 21px;
  font-size: 15px;
  color: #fff;
  background-color: #333333;
  border: 0;
  cursor: pointer;
  transition: background-color 0.25s ease-in;
}
.btn:hover {
  background-color: #1a1c19;
}
.btn:focus {
  outline: 0;
}
.tetris__block-controls .btn:first-child,
.tetris__game-controls .btn:first-child {
  border-top-left-radius: 4px;
  border-bottom-left-radius: 4px;
}
.tetris__block-controls .btn:not(:first-child),
.tetris__game-controls .btn:not(:first-child) {
  border-left: 1px solid #1a1c19;
}
.tetris__block-controls .btn:last-child,
.tetris__game-controls .btn:last-child {
  border-top-right-radius: 4px;
  border-bottom-right-radius: 4px;
}


