<template>
  <div>
    <h1 v-if="winner == 'Draw'">{{ winner }}</h1>
    <h1 v-else-if="winner == 'O' || winner == 'X'">Winner is {{ winner }}</h1>
    <div class="gameboard">
      <table>
        <tbody>
          <tr v-for="(matrix, indexMatrix) in matrixs" :key="matrix.name">
            <td v-for="(data, indexItem) in matrix" :key="data">
              <button @click="userTurn(indexMatrix, indexItem)">
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
      latestComputerMove: {
        index: 0,
      },
      latestUserMove: {
        indexMatrix: 0,
        indexItem: 0,
      },
      firstUserMove: 0,
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
    this.fillBoard(8);
  },
  watch: {
    matrixs: {
      handler() {
        this.compTurn(this.move);
      },
      deep: true,
    },
  },
  methods: {
    reverseTurn() {
      this.cross = !this.cross;
      this.circle = !this.circle;
    },
    stopGame() {
      this.start = false;
      this.stop = true;
    },
    startOver() {
      location.reload();
    },
    compTurn(move) {
      if (this.stop == false) {
        let { userIndex } = this.searchIndex;
        switch (move) {
          case 7:
            parseInt(userIndex) + 1 == 8
              ? this.fillBoard(this.moveSanitazion(userIndex + 2))
              : this.fillBoard(this.moveSanitazion(userIndex) + 1);
            this.firstUserMove = parseInt(userIndex);
            break;
          case 5: {
            if (
              this.moveSanitazion(parseInt(userIndex)) ==
              this.moveSanitazion(this.latestComputerMove.index + 4)
            ) {
              this.fillBoard(
                this.moveSanitazion(this.latestComputerMove.index + 2)
              );
            } else {
              this.winner = "O";
              this.fillBoard(
                this.moveSanitazion(this.latestComputerMove.index + 4)
              );
            }
            break;
          }
          case 3: {
            if (
              this.moveSanitazion(parseInt(userIndex)) ==
              this.moveSanitazion(this.latestComputerMove.index + 4)
            ) {
              if (this.firstUserMove % 2 == 0) {
                this.fillBoard(
                  this.moveSanitazion(this.latestComputerMove.index + 3)
                );
              } else {
                this.winner = "O";
                this.fillBoard(
                  this.moveSanitazion(this.latestComputerMove.index + 7)
                );
              }
            } else {
              this.winner = "O";
              this.fillBoard(
                this.moveSanitazion(this.latestComputerMove.index + 4)
              );
            }
            break;
          }
          case 1: {
            if (
              this.moveSanitazion(parseInt(userIndex)) ==
              this.moveSanitazion(this.latestComputerMove.index + 4)
            ) {
              this.winner = "Draw";
              this.fillBoard(
                this.moveSanitazion(this.latestComputerMove.index + 6)
              );
            } else {
              this.winner = "O";
              this.fillBoard(
                this.moveSanitazion(this.latestComputerMove.index + 4)
              );
            }
            break;
          }
          default:
            alert("Something went wrong");
            this.stopGame();
            break;
        }
      }
    },
    fillBoard(index) {
      if (this.start) {
        const indexMatrix = this.naturalizationData[index].indexMatrix;
        const indexItem = this.naturalizationData[index].indexItem;
        if (this.circle) {
          this.matrixs[indexMatrix][indexItem].details.value = "O";
          this.latestComputerMove.index = index;
        } else if (this.cross) {
          this.matrixs[indexMatrix][indexItem].details.value = "X";
        }

        this.move -= 1;
        this.winningNotification();
        this.reverseTurn();
      }
    },
    userTurn(indexMatrix, indexItem) {
      this.latestUserMove.indexMatrix = indexMatrix;
      this.latestUserMove.indexItem = indexItem;
      let { userIndex } = this.searchIndex;

      this.fillBoard(userIndex);
    },
    moveSanitazion(number) {
      return parseInt(number) % 8;
    },
    winningNotification() {
      if (this.winner == "O") {
        alert("Winner is " + this.winner);
        this.stopGame();
      } else if (this.winner == "Draw") {
        alert(this.winner);
        this.stopGame();
      }
    },
  },
  computed: {
    searchIndex() {
      let userIndex;
      for (let item in this.naturalizationData) {
        if (
          this.latestUserMove.indexMatrix ==
            this.naturalizationData[item].indexMatrix &&
          this.latestUserMove.indexItem ==
            this.naturalizationData[item].indexItem
        ) {
          userIndex = item;
        }
      }
      return { userIndex };
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
