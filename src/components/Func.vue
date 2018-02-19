<script>
import _ from "lodash";
import arraylevel from "../assets/level.json";
export default {
  data() {
    return {
      arraynumber: []
    };
  },
  methods: {
    randomIntFromInterval(min, max) {
      return Math.floor(Math.random() * (max - min + 1) + min);
    },
    getRamdomBoardFor(array) {
      var idx = this.randomIntFromInterval(0, array.length - 1);
      return array[idx];
    },
    newCell(id, i, j, value, editable) {
      return {
        id,
        i,
        j,
        value,
        editable,
        hasConflict: false
      };
    },
    genBoard(level) {
      this.arraynumber = Array.from(this.getRamdomBoardFor(arraylevel[level]));
      var array = [];
      for (var i = 0; i < 81; i++) {
        if (this.arraynumber[i] === "0") {
          array.push(null);
        } else {
          array.push(parseInt(this.arraynumber[i]));
        }
      }
      array = _.chunk(array, 9);
      var inc = 0;
      var game = [];
      for (var i = 0; i < 9; i++) {
        var line = [];
        for (var j = 0; j < 9; j++) {
          line.push(
            this.newCell(
              (inc = inc + 1),
              i,
              j,
              array[i][j],
              array[i][j] === null
            )
          );
        }
        game.push(line);
      }
      this.checkConflicts(game);
      return game;
    },
    markAllWithoutConflict(cells) {
      for (var i = 0; i < 9; i++) {
        for (var j = 0; j < 9; j++) {
          cells[i][j].hasConflict = false;
        }
      }
    },
    checkSubset(array) {
      //var nums = {};
      for (let i = 0; i < 9; i++) {
        if (array[i].value !== null) {
          array[i].hasConflict = true;
        }
        //nums[array[i].value] = i;
      }
    },
    checksquare(arr, matrix, cell, i) {
      if (cell.j < 3) {
        for (let j = 0; j < 3; j++) {
          if (matrix[i][j].hasConflict) {
            arr.push(matrix[i][j].value);
          }
        }
      }
      if (cell.j > 2 && cell.j < 6) {
        for (let j = 3; j < 6; j++) {
          if (matrix[i][j].hasConflict) {
            arr.push(matrix[i][j].value);
          }
        }
      }
      if (cell.j > 5 && cell.j < 9) {
        for (let j = 6; j < 9; j++) {
          if (matrix[i][j].hasConflict) {
            arr.push(matrix[i][j].value);
          }
        }
      }
      return arr;
    },
    checkrepeatnumber(matrix, cell) {
      var arr = [];
      for (let i = 0; i < 9; i++) {
        if (matrix[i][cell.j].hasConflict) {
          arr.push(matrix[i][cell.j].value);
        }
      }
      for (let j = 0; j < 9; j++) {
        if (matrix[cell.i][j].hasConflict) {
          arr.push(matrix[cell.i][j].value);
        }
      }
      if (cell.i < 3) {
        for (let i = 0; i < 3; i++) {
          this.checksquare(arr, matrix, cell, i);
        }
      }
      if (cell.i > 2 && cell.i < 6) {
        for (let i = 3; i < 6; i++) {
          this.checksquare(arr, matrix, cell, i);
        }
      }
      if (cell.i > 5 && cell.i < 9) {
        for (let i = 6; i < 9; i++) {
          this.checksquare(arr, matrix, cell, i);
        }
      }

      arr = arr.filter(function(item, i) {
        return arr.indexOf(item) == i;
      });
      console.log(arr);
      return arr;
    },

    checkConflicts(cells) {
      //this.markAllWithoutConflict(cells);
      // check horizontal lines
      for (let i = 0; i < 9; i++) {
        var arr = [];
        for (let j = 0; j < 9; j++) {
          arr.push(cells[i][j]);
        }
        this.checkSubset(arr);
      }

      // check vertical lines
      for (let j = 0; j < 9; j++) {
        var arr = [];
        for (let i = 0; i < 9; i++) {
          arr.push(cells[i][j]);
        }
        this.checkSubset(arr);
      }

      // check squares
      var c = cells;
      this.checkSubset([
        c[0][0],
        c[0][1],
        c[0][2],
        c[1][0],
        c[1][1],
        c[1][2],
        c[2][0],
        c[2][1],
        c[2][2]
      ]);
      this.checkSubset([
        c[3][0],
        c[3][1],
        c[3][2],
        c[4][0],
        c[4][1],
        c[4][2],
        c[5][0],
        c[5][1],
        c[5][2]
      ]);
      this.checkSubset([
        c[6][0],
        c[6][1],
        c[6][2],
        c[7][0],
        c[7][1],
        c[7][2],
        c[8][0],
        c[8][1],
        c[8][2]
      ]);

      this.checkSubset([
        c[0][3],
        c[0][4],
        c[0][5],
        c[1][3],
        c[1][4],
        c[1][5],
        c[2][3],
        c[2][4],
        c[2][5]
      ]);
      this.checkSubset([
        c[3][3],
        c[3][4],
        c[3][5],
        c[4][3],
        c[4][4],
        c[4][5],
        c[5][3],
        c[5][4],
        c[5][5]
      ]);
      this.checkSubset([
        c[6][3],
        c[6][4],
        c[6][5],
        c[7][3],
        c[7][4],
        c[7][5],
        c[8][3],
        c[8][4],
        c[8][5]
      ]);

      this.checkSubset([
        c[0][6],
        c[0][7],
        c[0][8],
        c[1][6],
        c[1][7],
        c[1][8],
        c[2][6],
        c[2][7],
        c[2][8]
      ]);
      this.checkSubset([
        c[3][6],
        c[3][7],
        c[3][8],
        c[4][6],
        c[4][7],
        c[4][8],
        c[5][6],
        c[5][7],
        c[5][8]
      ]);
      this.checkSubset([
        c[6][6],
        c[6][7],
        c[6][8],
        c[7][6],
        c[7][7],
        c[7][8],
        c[8][6],
        c[8][7],
        c[8][8]
      ]);
      //console.log(JSON.stringify(cells, null, 2));
    }
  }
};
</script>
