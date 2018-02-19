<template>
  <div>
    <img v-show=!hideimage src="../assets/logo.png">
    <h1>Lazy Sudoku</h1>
    <p>New Game</p>
    <button @click="shuffle('easy')">
      (EASY)
    </button>
     <button @click="shuffle('medium')">
      (MEDIUM)
    </button>
     <button @click="shuffle('hard')">
      (HARD)  
    </button>
    <div class="demo">
      <transition-group name="cell" tag="div" class="container">
        <div v-for="length in matrix.length" :key="length" class="length" v-if="hideimage">
          <div v-for="cell in matrix[length - 1]" :key="cell.id" class="cell" >
             <button v-on:click="cellClick(cell)" :disabled="!cell.editable" :class="{cellwhite: cell.editable, highlight:cell.id == isClick}">
              {{ cell.value }}
            </button>
          </div> 
        </div> 
      </transition-group>
    </div>
    <div style="margin: 0 20% 0 20%;">
      <Chioce :is-show="hideimage" @clicked="selectNumber" :disablenumber="disablenumber"/> 
    </div>
  </div>
</template>

<script>
import _ from "lodash";

import Chioce from "../components/Chioce";
import Func from "../components/Func";
export default {
  data() {
    return {
      matrix: [],
      isSelect: false,
      hideimage: false,
      isClick: undefined,
      cell: [],
      disablenumber: []
    };
  },
  methods: {
    shuffle: function(level) {
      this.matrix = _.shuffle(Func.methods.genBoard(level));
      this.isSelect = false;
      this.isClick = undefined;
      this.hideimage = true;
    },
    cellClick: function(cell) {
      if (cell.editable) {
        this.isSelect = true;
        this.isClick = cell.id;
        this.cell = cell;
        //this.disablenumber = Func.methods.checkrepeatnumber(this.matrix, cell);
      } else {
        this.isSelect = false;
        this.isClick = "";
      }
    },
    selectNumber(value) {
      this.matrix.map((item, i) => {
        item.map((subitem, j) => {
          if (subitem.id == this.isClick) {
            subitem.value = value;
          }
        });
      });
    }
  },
  components: {
    Chioce,
    Func
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.demo {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
  margin-bottom: 20px;
}
.container {
  display: flex;
  justify-content: space-around;
  align-items: center;
  /* flex-wrap: wrap; */
  /* width: 330px; */
}
.cell button {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 40px;
  height: 40px;
  border: 1px solid #aaa;
}
.highlight {
  border: 2px red solid !important;
}
.cell:nth-child(3) {
  border-bottom: 1px red solid !important;
}
.cell:nth-child(6) {
  border-bottom: 1px red solid !important;
}
.length:nth-child(3) {
  border-right: 1px red solid !important;
}
.length:nth-child(6) {
  border-right: 1px red solid !important;
}
.cell :disabled {
  color: blue;
}
.cell-enter-active, .cell-leave-active {
  transition: opacity 1s;
}
.cell-enter, .cell-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}
.cell-move {
  transition: transform 1s;
}
.cellwhite {
  background-color: #fff;
}
</style>
