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
