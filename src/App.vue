<template>
  <div id="app">
    <div class="status">{{ status }}</div>
    <button v-on:click="reset" class="clear">Reset</button>
    <template v-for="row in 3">
      <div class="row" :key="row">
        <button
          :ref="(row - 1) * 3 + (button - 1)"
          v-for="button in 3"
          class="square"
          :key="button"
          v-on:click="OnClicked(row - 1, button - 1)"
        ></button>
      </div>
    </template>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      winner: null,
      isPlayerX: true,
      tics: Array(9).fill(""),
    };
  },
  computed: {
    status: function () {
      if (this.winner === "X" || this.winner === "0") {
        return "Winner is " + this.winner;
      }

      if (this.isPlayerX) {
        return "Next Player: X";
      } else {
        return "Next Player: 0";
      }
    },
  },

  methods: {
    OnClicked(row, button) {
      this.coordinate = row * 3 + button;

      if (this.winner === "X" || this.winner === "0") {
        return;
      }

      if (
        this.tics[this.coordinate] === "X" ||
        this.tics[this.coordinate] === "0"
      ) {
        return;
      }
      if (this.isPlayerX === true) {
        this.tics[this.coordinate] = "X";
        this.$refs[this.coordinate][0].innerText = "X";
      } else {
        this.tics[this.coordinate] = "0";
        this.$refs[this.coordinate][0].innerText = "0";
      }
      this.isPlayerX = !this.isPlayerX;
      this.calculateWinner();
    },

    calculateWinner() {
      const lines = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
      ];
      for (let i = 0; i < lines.length; i++) {
        const [a, b, c] = lines[i];
        if (
          this.tics[a] &&
          this.tics[a] === this.tics[b] &&
          this.tics[b] === this.tics[c]
        ) {
          this.winner = this.tics[a];
          return;
        }
      }
    },
    reset() {
      for (let i = 0; i < this.tics.length; i++) {
        this.tics[i] = "";
        this.$refs[i][0].innerText = "";
      }
      this.winner = null;
      this.isPlayerX = true;
    },
  },
};
</script>

<style scoped>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.status {
  background-color: #74ED16;
  color: #fff;
  border: 0px;
  border-radius: 10px;
  /* border-top-right-radius: 10px; */

  font-weight: bold;
  margin: 0px;
  padding: 15px;
  width: 95%;
  margin-bottom: 10px;
}

.status:hover {
  background-color: #64B427;
}

.clear {
  background-color: #e74c3c;
  color: #fff;
  border: 0px;
  border-radius: 10px;
  margin-bottom: 10px;
  font-weight: bold;
  padding: 15px;
  width: 95%;
}

.clear:hover {
  background-color: #c0392b;
  cursor: pointer;
}

.square {
  border: 2px solid #999;

  font-size: 80px;
  font-weight: bold;
  line-height: 68px;

  height: 100px;

  text-align: center;

  justify-content: space-around;
  align-items: center;
  width: 100px;

  background-color: #4A5367;
  color: #ffffff;
  overflow-x: none;
}
.row {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>