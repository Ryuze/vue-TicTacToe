<template>
  <div>
    <h1 v-if="winner == 'Draw'">{{ winner }}</h1>
    <h1 v-else>Winner is {{ winner }}</h1>
    <div class="gameboard">
      <table>
        <tbody>
          <tr v-for="(matrix, indexMatrix) in matrixs" :key="matrix">
            <td v-for="(data, index) in matrix" :key="data">
              <button
                @click="fillMatrix(indexMatrix, index)"
                :class="data.status == 'win' ? 'winning' : ''"
              >
                {{ data.value }}
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
  </div>
</template>

<script>
export default {
  data() {
    return {
      matrixs: [
        {
          a: {
            value: "",
            status: "",
          },
          b: {
            value: "",
            status: "",
          },
          c: {
            value: "",
            status: "",
          },
        },
        {
          d: {
            value: "",
            status: "",
          },
          e: {
            value: "",
            status: "",
          },
          f: {
            value: "",
            status: "",
          },
        },
        {
          g: {
            value: "",
            status: "",
          },
          h: {
            value: "",
            status: "",
          },
          i: {
            value: "",
            status: "",
          },
        },
      ],
      start: false,
      stop: false,
      cross: false,
      circle: true,
      move: 9,
      winner: "",
    };
  },
  methods: {
    reverseTurn() {
      this.cross = !this.cross;
      this.circle = !this.circle;
    },
    fillMatrix(indexMatrix, index) {
      if (this.stop == false) {
        if (this.circle) {
          if (this.matrixs[indexMatrix][index].value == "") {
            this.start = true;
            this.matrixs[indexMatrix][index].value = "O";
            this.move -= 1;
            this.winningCondition();
            this.reverseTurn();
          } else {
            alert("Invalid move, please choose unfilled spot only.");
          }
        } else if (this.cross) {
          if (this.matrixs[indexMatrix][index].value == "") {
            this.start = true;
            this.matrixs[indexMatrix][index].value = "X";
            this.move -= 1;
            this.winningCondition();
            this.reverseTurn();
          } else {
            alert("Invalid move, please choose unfilled spot only.");
          }
        } else {
          console.log("Something went wrong");
        }
      }
    },
    winningCondition() {
      if (this.start && this.move != 0) {
        if (this.checkCondition()) {
          alert("Winner is " + this.winner);
          this.changeColorWinner();
          this.stopGame();
        }
      } else {
        alert("Draw");
        this.winner = "Draw";
      }
    },
    stopGame() {
      this.start = false;
      this.stop = true;
    },
    changeColorWinner() {
      if (
        (this.matrixs[0]["a"].value == "O" &&
          this.matrixs[0]["b"].value == "O" &&
          this.matrixs[0]["c"].value == "O") ||
        (this.matrixs[0]["a"].value == "X" &&
          this.matrixs[0]["b"].value == "X" &&
          this.matrixs[0]["c"].value == "X")
      ) {
        this.matrixs[0]["a"].status = "win";
        this.matrixs[0]["b"].status = "win";
        this.matrixs[0]["c"].status = "win";
      } else if (
        (this.matrixs[1]["d"].value == "O" &&
          this.matrixs[1]["e"].value == "O" &&
          this.matrixs[1]["f"].value == "O") ||
        (this.matrixs[1]["d"].value == "X" &&
          this.matrixs[1]["e"].value == "X" &&
          this.matrixs[1]["f"].value == "X")
      ) {
        this.matrixs[1]["d"].status = "win";
        this.matrixs[1]["e"].status = "win";
        this.matrixs[1]["f"].status = "win";
      } else if (
        (this.matrixs[2]["g"].value == "O" &&
          this.matrixs[2]["h"].value == "O" &&
          this.matrixs[2]["i"].value == "O") ||
        (this.matrixs[2]["g"].value == "X" &&
          this.matrixs[2]["h"].value == "X" &&
          this.matrixs[2]["i"].value == "X")
      ) {
        this.matrixs[2]["g"].status = "win";
        this.matrixs[2]["h"].status = "win";
        this.matrixs[2]["i"].status = "win";
      } else if (
        (this.matrixs[0]["a"].value == "O" &&
          this.matrixs[1]["d"].value == "O" &&
          this.matrixs[2]["g"].value == "O") ||
        (this.matrixs[0]["a"].value == "X" &&
          this.matrixs[1]["d"].value == "X" &&
          this.matrixs[2]["g"].value == "X")
      ) {
        this.matrixs[0]["a"].status = "win";
        this.matrixs[1]["d"].status = "win";
        this.matrixs[2]["g"].status = "win";
      } else if (
        (this.matrixs[0]["b"].value == "O" &&
          this.matrixs[1]["e"].value == "O" &&
          this.matrixs[2]["h"].value == "O") ||
        (this.matrixs[0]["b"].value == "X" &&
          this.matrixs[1]["e"].value == "X" &&
          this.matrixs[2]["h"].value == "X")
      ) {
        this.matrixs[0]["b"].status = "win";
        this.matrixs[1]["e"].status = "win";
        this.matrixs[2]["h"].status = "win";
      } else if (
        (this.matrixs[0]["c"].value == "O" &&
          this.matrixs[1]["f"].value == "O" &&
          this.matrixs[2]["i"].value == "O") ||
        (this.matrixs[0]["c"].value == "X" &&
          this.matrixs[1]["f"].value == "X" &&
          this.matrixs[2]["i"].value == "X")
      ) {
        this.matrixs[0]["c"].status = "win";
        this.matrixs[1]["f"].status = "win";
        this.matrixs[2]["i"].status = "win";
      } else if (
        (this.matrixs[0]["a"].value == "O" &&
          this.matrixs[1]["e"].value == "O" &&
          this.matrixs[2]["i"].value == "O") ||
        (this.matrixs[0]["a"].value == "X" &&
          this.matrixs[1]["e"].value == "X" &&
          this.matrixs[2]["i"].value == "X")
      ) {
        this.matrixs[0]["a"].status = "win";
        this.matrixs[1]["e"].status = "win";
        this.matrixs[2]["i"].status = "win";
      } else if (
        (this.matrixs[0]["c"].value == "O" &&
          this.matrixs[1]["e"].value == "O" &&
          this.matrixs[2]["g"].value == "O") ||
        (this.matrixs[0]["c"].value == "X" &&
          this.matrixs[1]["e"].value == "X" &&
          this.matrixs[2]["g"].value == "X")
      ) {
        this.matrixs[0]["c"].status = "win";
        this.matrixs[1]["e"].status = "win";
        this.matrixs[2]["g"].status = "win";
      }
    },
    checkCondition() {
      if (
        this.matrixs[0]["a"].value == "O" &&
        this.matrixs[0]["b"].value == "O" &&
        this.matrixs[0]["c"].value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[1]["d"].value == "O" &&
        this.matrixs[1]["e"].value == "O" &&
        this.matrixs[1]["f"].value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[2]["g"].value == "O" &&
        this.matrixs[2]["h"].value == "O" &&
        this.matrixs[2]["i"].value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0]["a"].value == "O" &&
        this.matrixs[1]["d"].value == "O" &&
        this.matrixs[2]["g"].value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0]["b"].value == "O" &&
        this.matrixs[1]["e"].value == "O" &&
        this.matrixs[2]["h"].value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0]["c"].value == "O" &&
        this.matrixs[1]["f"].value == "O" &&
        this.matrixs[2]["i"].value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0]["a"].value == "O" &&
        this.matrixs[1]["e"].value == "O" &&
        this.matrixs[2]["i"].value == "O"
      ) {
        this.winner = "O";
        return true;
      } else if (
        this.matrixs[0]["c"].value == "O" &&
        this.matrixs[1]["e"].value == "O" &&
        this.matrixs[2]["g"].value == "O"
      ) {
        this.winner = "O";
        return true;
      }

      if (
        this.matrixs[0]["a"].value == "X" &&
        this.matrixs[0]["b"].value == "X" &&
        this.matrixs[0]["c"].value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[1]["d"].value == "X" &&
        this.matrixs[1]["e"].value == "X" &&
        this.matrixs[1]["f"].value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[2]["g"].value == "X" &&
        this.matrixs[2]["h"].value == "X" &&
        this.matrixs[2]["i"].value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0]["a"].value == "X" &&
        this.matrixs[1]["d"].value == "X" &&
        this.matrixs[2]["g"].value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0]["b"].value == "X" &&
        this.matrixs[1]["e"].value == "X" &&
        this.matrixs[2]["h"].value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0]["c"].value == "X" &&
        this.matrixs[1]["f"].value == "X" &&
        this.matrixs[2]["i"].value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0]["a"].value == "X" &&
        this.matrixs[1]["e"].value == "X" &&
        this.matrixs[2]["i"].value == "X"
      ) {
        this.winner = "X";
        return true;
      } else if (
        this.matrixs[0]["c"].value == "X" &&
        this.matrixs[1]["e"].value == "X" &&
        this.matrixs[2]["g"].value == "X"
      ) {
        this.winner = "X";
        return true;
      }
    },
    startOver() {
      location.reload();
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
