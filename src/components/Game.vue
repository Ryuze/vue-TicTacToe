<template>
  <div>
    <h1>{{ winner }}</h1>
    <div class="gameboard">
      <table>
        <tbody>
          <tr v-for="(matrix, indexMatrix) in matrixs" :key="matrix">
            <td v-for="(data, index) in matrix" :key="data">
              <button @click="fillMatrix(indexMatrix, index)">
                {{ data }}
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
    <div>
      <button @click="startOver" style="background-color: green; color: white; margin-top: 1em; border-radius: 20%">Reload</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      matrixs: [
        {
          a: "",
          b: "",
          c: "",
        },
        {
          d: "",
          e: "",
          f: "",
        },
        {
          g: "",
          h: "",
          i: "",
        },
      ],
      start: false,
      stop: false,
      cross: false,
      circle: true,
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
          this.start = true;
          this.matrixs[indexMatrix][index] = "O";
          this.winningCondition();
          this.reverseTurn();
        } else if (this.cross) {
          this.start = true;
          this.matrixs[indexMatrix][index] = "X";
          this.winningCondition();
          this.reverseTurn();
        } else {
          console.log("Something went wrong");
        }
      }
    },
    winningCondition() {
      if (this.start) {
        if (
          this.matrixs[0]["a"] == "O" &&
          this.matrixs[0]["b"] == "O" &&
          this.matrixs[0]["c"] == "O"
        ) {
          alert("Winner is O");
          this.winner = "Winner is O";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[1]["d"] == "O" &&
          this.matrixs[1]["e"] == "O" &&
          this.matrixs[1]["f"] == "O"
        ) {
          alert("Winner is O");
          this.winner = "Winner is O";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[2]["g"] == "O" &&
          this.matrixs[2]["h"] == "O" &&
          this.matrixs[2]["i"] == "O"
        ) {
          alert("Winner is O");
          this.winner = "Winner is O";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["a"] == "O" &&
          this.matrixs[1]["d"] == "O" &&
          this.matrixs[2]["g"] == "O"
        ) {
          alert("Winner is O");
          this.winner = "Winner is O";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["b"] == "O" &&
          this.matrixs[1]["e"] == "O" &&
          this.matrixs[2]["h"] == "O"
        ) {
          alert("Winner is O");
          this.winner = "Winner is O";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["c"] == "O" &&
          this.matrixs[1]["f"] == "O" &&
          this.matrixs[2]["i"] == "O"
        ) {
          alert("Winner is O");
          this.winner = "Winner is O";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["a"] == "O" &&
          this.matrixs[1]["e"] == "O" &&
          this.matrixs[2]["i"] == "O"
        ) {
          alert("Winner is O");
          this.winner = "Winner is O";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["c"] == "O" &&
          this.matrixs[1]["e"] == "O" &&
          this.matrixs[2]["g"] == "O"
        ) {
          alert("Winner is O");
          this.winner = "Winner is O";
          this.start = false;
          this.stop = true;
        }

        if (
          this.matrixs[0]["a"] == "X" &&
          this.matrixs[0]["b"] == "X" &&
          this.matrixs[0]["c"] == "X"
        ) {
          alert("Winner is X");
          this.winner = "Winner is X";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[1]["d"] == "X" &&
          this.matrixs[1]["e"] == "X" &&
          this.matrixs[1]["f"] == "X"
        ) {
          alert("Winner is X");
          this.winner = "Winner is X";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[2]["g"] == "X" &&
          this.matrixs[2]["h"] == "X" &&
          this.matrixs[2]["i"] == "X"
        ) {
          alert("Winner is X");
          this.winner = "Winner is X";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["a"] == "X" &&
          this.matrixs[1]["d"] == "X" &&
          this.matrixs[2]["g"] == "X"
        ) {
          alert("Winner is X");
          this.winner = "Winner is X";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["b"] == "X" &&
          this.matrixs[1]["e"] == "X" &&
          this.matrixs[2]["h"] == "X"
        ) {
          alert("Winner is X");
          this.winner = "Winner is X";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["c"] == "X" &&
          this.matrixs[1]["f"] == "X" &&
          this.matrixs[2]["i"] == "X"
        ) {
          alert("Winner is X");
          this.winner = "Winner is X";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["a"] == "X" &&
          this.matrixs[1]["e"] == "X" &&
          this.matrixs[2]["i"] == "X"
        ) {
          alert("Winner is X");
          this.winner = "Winner is X";
          this.start = false;
          this.stop = true;
        } else if (
          this.matrixs[0]["c"] == "X" &&
          this.matrixs[1]["e"] == "X" &&
          this.matrixs[2]["g"] == "X"
        ) {
          alert("Winner is X");
          this.winner = "Winner is X";
          this.start = false;
          this.stop = true;
        }
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
</style>
