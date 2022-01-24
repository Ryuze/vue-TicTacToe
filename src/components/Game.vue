<template>
  <div>
    <h1 v-if="winner == 'Draw'">{{ winner }}</h1>
    <h1 v-else-if="winner == 'O' || winner == 'X'">Winner is {{ winner }}</h1>
    <div class="gameboard">
      <table>
        <tbody>
          <!-- <tr v-for="(matrix, indexMatrix) in matrixs" :key="matrix">
            <td v-for="(data, index) in matrix" :key="data">
              <button
                @click="fillMatrix(indexMatrix, index)"
                :class="data.status == 'win' ? 'winning' : ''"
              >
                {{ data.value }}
              </button>
            </td>
          </tr> -->
          <tr v-for="(matrix, indexMatrix) in matrixs" :key="matrix.name">
            <td v-for="(data, index) in matrix" :key="data">
              <button
                @click="fillMatrix(indexMatrix, index)"
                :class="data.details.status == 'win' ? 'winning' : ''"
              >
                {{ data.details.value }}
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <button
        @click="startOver"
        style="
          background-color: green;
          color: white;
          margin-top: 1em;
          border-radius: 20%;
        "
      >
        Reload
      </button>
    </div>
    <div v-if="stop == false">
      <h3>Now is {{ circle ? "O" : cross ? "X" : "" }} turn</h3>
    </div>
    <div v-else-if="winner == 'Draw'">
      <h3>Draw? Let's try again to find out who is the true winner!</h3>
    </div>
    <div v-else>
      <h3>Congratulation</h3>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      matrixs: [
        [
          {
            name: "1",
            details: {
              value: "",
              status: "",
            },
          },
          {
            name: "2",
            details: {
              value: "",
              status: "",
            },
          },
          {
            name: "3",
            details: {
              value: "",
              status: "",
            },
          },
        ],
        [
          {
            name: "4",
            details: {
              value: "",
              status: "",
            },
          },
          {
            name: "5",
            details: {
              value: "",
              status: "",
            },
          },
          {
            name: "6",
            details: {
              value: "",
              status: "",
            },
          },
        ],
        [
          {
            name: "7",
            details: {
              value: "",
              status: "",
            },
          },
          {
            name: "8",
            details: {
              value: "",
              status: "",
            },
          },
          {
            name: "9",
            details: {
              value: "",
              status: "",
            },
          },
        ],
      ],
      start: false,
      stop: false,
      cross: false,
      circle: true,
      move: 9,
      firstPlayerMove: {
        indexMatrix: 0,
        indexItem: 0,
      },
      indexMatrix: 0,
      indexItem: 0,
      latestComputerMove: {
        indexMatrix: 0,
        indexItem: 0,
      },
      naturalizationData: [
        {
          indexMatrix: 0,
          indexItem: 0,
        },
        {
          indexMatrix: 0,
          indexItem: 1,
        },
        {
          indexMatrix: 0,
          indexItem: 2,
        },
        {
          indexMatrix: 1,
          indexItem: 2,
        },
        {
          indexMatrix: 2,
          indexItem: 2,
        },
        {
          indexMatrix: 2,
          indexItem: 1,
        },
        {
          indexMatrix: 2,
          indexItem: 0,
        },
        {
          indexMatrix: 1,
          indexItem: 0,
        },
        {
          indexMatrix: 1,
          indexItem: 1,
        },
      ],
      winner: "",
    };
  },
  mounted() {
    this.start = true;
    this.fillMatrix(1, 1);
  },
  watch: {
    matrixs: {
      handler() {
        this.computerTurn(this.move, this.indexMatrix, this.indexItem);
      },
      deep: true,
    },
  },
  methods: {
    reverseTurn() {
      this.cross = !this.cross;
      this.circle = !this.circle;
    },
    fillMatrix(indexMatrix, index) {
      if (this.stop == false) {
        this.start = true;
        if (this.matrixs[indexMatrix][index].details.value == "") {
          if (this.circle) {
            this.matrixs[indexMatrix][index].details.value = "O";
          } else if (this.cross) {
            this.matrixs[indexMatrix][index].details.value = "X";
          } else {
            console.log("Something went wrong");
          }

          this.move -= 1;
          this.indexMatrix = indexMatrix;
          this.indexItem = index;
          this.winningCondition();
          this.reverseTurn();
        } else {
          alert("Invalid move, please choose unfilled spot only.");
        }
      }
    },
    winningCondition() {
      if (this.move >= 0) {
        if (this.checkCondition()) {
          alert("Winner is " + this.winner);
          this.changeColorWinner();
          this.stopGame();
        } else if (this.move == 0 && this.winner == "") {
          alert("Draw");
          this.winner = "Draw";
          this.stopGame();
        }
      }
    },
    stopGame() {
      this.start = false;
      this.stop = true;
    },
    changeColorWinner() {
      if (
        (this.matrixs[0][0].details.value == "O" &&
          this.matrixs[0][1].details.value == "O" &&
          this.matrixs[0][2].details.value == "O") ||
        (this.matrixs[0][0].details.value == "X" &&
          this.matrixs[0][1].details.value == "X" &&
          this.matrixs[0][2].details.value == "X")
      ) {
        this.matrixs[0][0].details.status = "win";
        this.matrixs[0][1].details.status = "win";
        this.matrixs[0][2].details.status = "win";
      } else if (
        (this.matrixs[1][0].details.value == "O" &&
          this.matrixs[1][1].details.value == "O" &&
          this.matrixs[1][2].details.value == "O") ||
        (this.matrixs[1][0].details.value == "X" &&
          this.matrixs[1][1].details.value == "X" &&
          this.matrixs[1][2].details.value == "X")
      ) {
        this.matrixs[1][0].details.status = "win";
        this.matrixs[1][1].details.status = "win";
        this.matrixs[1][2].details.status = "win";
      } else if (
        (this.matrixs[2][0].details.value == "O" &&
          this.matrixs[2][1].details.value == "O" &&
          this.matrixs[2][2].details.value == "O") ||
        (this.matrixs[2][0].details.value == "X" &&
          this.matrixs[2][1].details.value == "X" &&
          this.matrixs[2][2].details.value == "X")
      ) {
        this.matrixs[2][0].details.status = "win";
        this.matrixs[2][1].details.status = "win";
        this.matrixs[2][2].details.status = "win";
      } else if (
        (this.matrixs[0][0].details.value == "O" &&
          this.matrixs[1][0].details.value == "O" &&
          this.matrixs[2][0].details.value == "O") ||
        (this.matrixs[0][0].details.value == "X" &&
          this.matrixs[1][0].details.value == "X" &&
          this.matrixs[2][0].details.value == "X")
      ) {
        this.matrixs[0][0].details.status = "win";
        this.matrixs[1][0].details.status = "win";
        this.matrixs[2][0].details.status = "win";
      } else if (
        (this.matrixs[0][1].details.value == "O" &&
          this.matrixs[1][1].details.value == "O" &&
          this.matrixs[2][1].details.value == "O") ||
        (this.matrixs[0][1].details.value == "X" &&
          this.matrixs[1][1].details.value == "X" &&
          this.matrixs[2][1].details.value == "X")
      ) {
        this.matrixs[0][1].details.status = "win";
        this.matrixs[1][1].details.status = "win";
        this.matrixs[2][1].details.status = "win";
      } else if (
        (this.matrixs[0][2].details.value == "O" &&
          this.matrixs[1][2].details.value == "O" &&
          this.matrixs[2][2].details.value == "O") ||
        (this.matrixs[0][2].details.value == "X" &&
          this.matrixs[1][2].details.value == "X" &&
          this.matrixs[2][2].details.value == "X")
      ) {
        this.matrixs[0][2].details.status = "win";
        this.matrixs[1][2].details.status = "win";
        this.matrixs[2][2].details.status = "win";
      } else if (
        (this.matrixs[0][0].details.value == "O" &&
          this.matrixs[1][1].details.value == "O" &&
          this.matrixs[2][2].details.value == "O") ||
        (this.matrixs[0][0].details.value == "X" &&
          this.matrixs[1][1].details.value == "X" &&
          this.matrixs[2][2].details.value == "X")
      ) {
        this.matrixs[0][0].details.status = "win";
        this.matrixs[1][1].details.status = "win";
        this.matrixs[2][2].details.status = "win";
      } else if (
        (this.matrixs[0][2].details.value == "O" &&
          this.matrixs[1][1].details.value == "O" &&
          this.matrixs[2][0].details.value == "O") ||
        (this.matrixs[0][2].details.value == "X" &&
          this.matrixs[1][1].details.value == "X" &&
          this.matrixs[2][0].details.value == "X")
      ) {
        this.matrixs[0][2].details.status = "win";
        this.matrixs[1][1].details.status = "win";
        this.matrixs[2][0].details.status = "win";
      }
    },
    checkCondition() {
      if (
        this.matrixs[0][0].details.value == "O" &&
        this.matrixs[0][1].details.value == "O" &&
        this.matrixs[0][2].details.value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[1][0].details.value == "O" &&
        this.matrixs[1][1].details.value == "O" &&
        this.matrixs[1][2].details.value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[2][0].details.value == "O" &&
        this.matrixs[2][1].details.value == "O" &&
        this.matrixs[2][2].details.value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0][0].details.value == "O" &&
        this.matrixs[1][0].details.value == "O" &&
        this.matrixs[2][0].details.value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0][1].details.value == "O" &&
        this.matrixs[1][1].details.value == "O" &&
        this.matrixs[2][1].details.value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0][2].details.value == "O" &&
        this.matrixs[1][2].details.value == "O" &&
        this.matrixs[2][2].details.value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0][0].details.value == "O" &&
        this.matrixs[1][1].details.value == "O" &&
        this.matrixs[2][2].details.value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0][2].details.value == "O" &&
        this.matrixs[1][1].details.value == "O" &&
        this.matrixs[2][0].details.value == "O"
      ) {
        this.winner = "O";
        return true;
      }

      if (
        this.matrixs[0][0].details.value == "X" &&
        this.matrixs[0][1].details.value == "X" &&
        this.matrixs[0][2].details.value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[1][0].details.value == "X" &&
        this.matrixs[1][1].details.value == "X" &&
        this.matrixs[1][2].details.value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[2][0].details.value == "X" &&
        this.matrixs[2][1].details.value == "X" &&
        this.matrixs[2][2].details.value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0][0].details.value == "X" &&
        this.matrixs[1][0].details.value == "X" &&
        this.matrixs[2][0].details.value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0][1].details.value == "X" &&
        this.matrixs[1][1].details.value == "X" &&
        this.matrixs[2][1].details.value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0][2].details.value == "X" &&
        this.matrixs[1][2].details.value == "X" &&
        this.matrixs[2][2].details.value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0][0].details.value == "X" &&
        this.matrixs[1][1].details.value == "X" &&
        this.matrixs[2][2].details.value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0][2].details.value == "X" &&
        this.matrixs[1][1].details.value == "X" &&
        this.matrixs[2][0].details.value == "X"
      ) {
        this.winner = "X";
        return true;
      }
    },
    startOver() {
      location.reload();
    },
    computerTurn(move, indexMatrix, indexItem) {
      if (this.stop == false) {
        switch (move) {
          case 7: {
            let userNaturalizationIndex = 0;
            this.firstPlayerMove.indexMatrix = this.indexMatrix;
            this.firstPlayerMove.indexItem = this.indexItem;

            for (let item in this.naturalizationData) {
              if (
                indexMatrix == this.naturalizationData[item].indexMatrix &&
                indexItem == this.naturalizationData[item].indexItem
              ) {
                userNaturalizationIndex = item;
              }
            }

            let sanitization = this.moveSanitazion(userNaturalizationIndex) + 1;

            if (sanitization == 8) {
              sanitization = 0;
            }

            const newIndexMatrix =
              this.naturalizationData[sanitization].indexMatrix;
            const newIndexItem =
              this.naturalizationData[sanitization].indexItem;

            this.fillMatrix(newIndexMatrix, newIndexItem);
            this.latestComputerMove.indexMatrix = newIndexMatrix;
            this.latestComputerMove.indexItem = newIndexItem;

            break;
          }
          case 5: {
            let userNaturalizationIndex = 0;
            let computerNaturalizationIndex = 0;

            for (let item in this.naturalizationData) {
              if (
                this.indexMatrix == this.naturalizationData[item].indexMatrix &&
                this.indexItem == this.naturalizationData[item].indexItem
              ) {
                userNaturalizationIndex = item;
              }

              if (
                this.latestComputerMove.indexMatrix ==
                  this.naturalizationData[item].indexMatrix &&
                this.latestComputerMove.indexItem ==
                  this.naturalizationData[item].indexItem
              ) {
                computerNaturalizationIndex = item;
              }
            }

            let userSanitization = this.moveSanitazion(userNaturalizationIndex);
            let computerSanitization = this.moveSanitazion(
              computerNaturalizationIndex
            );

            if (
              userSanitization == this.moveSanitazion(computerSanitization + 4)
            ) {
              computerSanitization += 2;

              if (computerSanitization >= 8) {
                computerSanitization %= 8;
              }

              const newIndexMatrix =
                this.naturalizationData[computerSanitization].indexMatrix;
              const newIndexItem =
                this.naturalizationData[computerSanitization].indexItem;

              this.fillMatrix(newIndexMatrix, newIndexItem);
              this.latestComputerMove.indexMatrix = newIndexMatrix;
              this.latestComputerMove.indexItem = newIndexItem;
            } else {
              computerSanitization += 4;

              if (computerSanitization >= 8) {
                computerSanitization %= 8;
              }

              const newIndexMatrix =
                this.naturalizationData[computerSanitization].indexMatrix;
              const newIndexItem =
                this.naturalizationData[computerSanitization].indexItem;

              this.fillMatrix(newIndexMatrix, newIndexItem);
              this.latestComputerMove.indexMatrix = newIndexMatrix;
              this.latestComputerMove.indexItem = newIndexItem;
            }

            break;
          }
          case 3: {
            let computerNaturalizationIndex = 0;
            let firstUserNaturalizationIndex = 0;

            for (let item in this.naturalizationData) {
              if (
                this.latestComputerMove.indexMatrix ==
                  this.naturalizationData[item].indexMatrix &&
                this.latestComputerMove.indexItem ==
                  this.naturalizationData[item].indexItem
              ) {
                computerNaturalizationIndex = item;
              }

              if (
                this.firstPlayerMove.indexMatrix ==
                  this.naturalizationData[item].indexMatrix &&
                this.firstPlayerMove.indexItem ==
                  this.naturalizationData[item].indexItem
              ) {
                firstUserNaturalizationIndex = item;
              }
            }

            let computerSanitization = this.moveSanitazion(
              computerNaturalizationIndex
            );
            let firstUserSanitization = this.moveSanitazion(
              firstUserNaturalizationIndex
            );

            if (firstUserSanitization % 2 == 0) {
              computerSanitization += 3;

              if (computerSanitization >= 8) {
                computerSanitization %= 8;
              }

              const newIndexMatrix =
                this.naturalizationData[computerSanitization].indexMatrix;
              const newIndexItem =
                this.naturalizationData[computerSanitization].indexItem;

              this.fillMatrix(newIndexMatrix, newIndexItem);
              this.latestComputerMove.indexMatrix = newIndexMatrix;
              this.latestComputerMove.indexItem = newIndexItem;
            } else {
              computerSanitization += 7;
              console.log(computerSanitization);

              if (computerSanitization >= 8) {
                computerSanitization %= 8;
                console.log(computerSanitization);
              }

              const newIndexMatrix =
                this.naturalizationData[computerSanitization].indexMatrix;
              const newIndexItem =
                this.naturalizationData[computerSanitization].indexItem;

              this.fillMatrix(newIndexMatrix, newIndexItem);
              this.latestComputerMove.indexMatrix = newIndexMatrix;
              this.latestComputerMove.indexItem = newIndexItem;
            }

            break;
          }
          case 1: {
            let userNaturalizationIndex = 0;
            let computerNaturalizationIndex = 0;

            for (let item in this.naturalizationData) {
              if (
                this.indexMatrix == this.naturalizationData[item].indexMatrix &&
                this.indexItem == this.naturalizationData[item].indexItem
              ) {
                userNaturalizationIndex = item;
              }

              if (
                this.latestComputerMove.indexMatrix ==
                  this.naturalizationData[item].indexMatrix &&
                this.latestComputerMove.indexItem ==
                  this.naturalizationData[item].indexItem
              ) {
                computerNaturalizationIndex = item;
              }
            }

            let userSanitization = this.moveSanitazion(userNaturalizationIndex);
            let computerSanitization = this.moveSanitazion(
              computerNaturalizationIndex
            );

            if (
              userSanitization == this.moveSanitazion(computerSanitization + 4)
            ) {
              computerSanitization += 6;

              if (computerSanitization >= 8) {
                computerSanitization %= 8;
              }

              const newIndexMatrix =
                this.naturalizationData[computerSanitization].indexMatrix;
              const newIndexItem =
                this.naturalizationData[computerSanitization].indexItem;

              this.fillMatrix(newIndexMatrix, newIndexItem);
              this.latestComputerMove.indexMatrix = newIndexMatrix;
              this.latestComputerMove.indexItem = newIndexItem;
            } else {
              computerSanitization += 4;

              if (computerSanitization >= 8) {
                computerSanitization %= 8;
              }

              const newIndexMatrix =
                this.naturalizationData[computerSanitization].indexMatrix;
              const newIndexItem =
                this.naturalizationData[computerSanitization].indexItem;

              this.fillMatrix(newIndexMatrix, newIndexItem);
              this.latestComputerMove.indexMatrix = newIndexMatrix;
              this.latestComputerMove.indexItem = newIndexItem;
            }

            break;
          }
        }
      }
    },
    moveSanitazion(number) {
      return parseInt(number) % 8;
    },
  },
};
</script>

<style lang="postcss" scoped>
table,
td {
  border: 2px solid black;
}

button {
  height: 5em;
  width: 5em;
  background-color: white;
  border: none;
  cursor: pointer;
}

.gameboard {
  display: flex;
  align-items: center;
  justify-content: center;
}

.winning {
  background-color: green;
  color: white;
}
</style>
