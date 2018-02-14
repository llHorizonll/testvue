<template>
  <div>
    <h1>Lazy Sudoku</h1>
    <p>Keep hitting the shuffle button until you win.</p>
    <button @click="shuffle('easy')">
      Easy
    </button>
    <div class="demo">
    <transition-group name="cell" tag="div" class="container">
      <div v-for="cell in cells" :key="cell.id" class="cell">
        <button v-on:click="cell.number += 1">
          {{ cell.number }}
        </button>
      </div>
    </transition-group>
    </div>
  </div>
</template>

<script>
import _ from "lodash";
import arraylevel from "../assets/level.json";
export default {
  data() {
    return {
      cells: Array.apply(null, { length: 81 }).map(function(_, index) {
        return {
          id: index,
          number: index % 9 + 1
        };
      })
    };
  },
  methods: {
    shuffle: function(level) {
      switch (level) {
        case "easy":
          function randomIntFromInterval(min, max) {
            return Math.floor(Math.random() * (max - min + 1) + min);
          }

          function getRamdomBoardFor(array) {
            var idx = randomIntFromInterval(0, array.length - 1);
            return array[idx];
          }
          this.cells = _.shuffle(this.cells);
          let arraynumber = Array.from(getRamdomBoardFor(arraylevel.easy));
          arraynumber.map((item, i) => {
            item == 0
              ? (this.cells[i].number = "")
              : (this.cells[i].number = item);
          });
          break;
        default:
          
          break;
      }
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.demo {
  display: flex;
  align-items: center;
  justify-content: center;
  height: 300px;
}
.container {
  display: flex;
  flex-wrap: wrap;
  width: 238px;
  margin-top: 10px;
}
.cell button {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 25px;
  height: 25px;
  border: 1px solid #aaa;
  margin-right: -1px;
  margin-bottom: -1px;
}
.cell:nth-child(3n) {
  margin-right: 0;
}
.cell:nth-child(27n) {
  margin-bottom: 0;
}
.cell-move {
  transition: transform 1s;
}
</style>
