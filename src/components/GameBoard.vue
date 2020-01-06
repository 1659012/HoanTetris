<template>
  <div id="gameBoard" ref="gameBoard">
    <h3>{{ dropRow }}</h3>
  </div>
</template>

<script>
export default {
  name: "GameBoard",
  props: [],
  components: {},
  data() {
    return {
      maxRow: 20,
      maxCol: 10,
      currentRow: 0,
      currentCol: 0,
      dropRow: 0,
      shapes: {
        IShape: [[1, 0], [3, 1], [2, 3], [0, 2]],
        JShape: [[1, 0], [2, 1], [1, 2], [2, 0]],

        //LShape: [[0, 1, 0], [0, 1, 0], [0, 1, 1]],
        LShape: [[0, 1, 0], [0, 1, 0], [0, 1, 1]],

        OSquare: [[1, 1, 0], [1, 1, 0], [0, 0, 0]],
        SShape: [[0, 2], [0, 0], [2, 0], [2, 2]],
        TShape: [[0, 1], [1, 0], [2, 1], [1, 2]],
        ZShape: [[0, 1], [1, 0], [2, 1], [1, 2]]
      }
    };
  },
  mounted() {
    this.initialize();
  },
  computed: {},
  watch: {},
  methods: {
    initialize() {
      new Promise(resolve => {
        this.createNewBoard();
        resolve();
      }).then(() => {
        this.runGame();
      });
    },

    createNewBoard() {
      var gameBoard = this.$refs.gameBoard;
      for (let row = 0; row < this.maxRow; row++) {
        for (let col = 0; col < this.maxCol; col++) {
          let node = document.createElement("div");
          node.setAttribute("class", "boardCell");
          node.setAttribute("id", row + "-" + col);
          gameBoard.appendChild(node);
        }
      }
    },

    dropShape() {
      if (this.dropRow < this.maxRow) {
        this.dropRow++;
      } else {
        this.dropRow = 0;
      }
    },

    drawShape(row, col, shapeType) {
      // //clean previous shape
      // var cells = document.getElementsByClassName("boardCell activeCell");
      // cells.forEach(cell => {
      //   cell.classList.remove("activeCell");
      // });

      // //render shape
      // var count = 0;
      // for (let i = 0; i < 3; i++) {
      //   for (let j = 0; j < 3; j++) {
      //     let cellRow = row + i;
      //     let cellCol = col + j;
      //     console.log(
      //       "cell " + cellRow + "-" + cellCol + "= " + this.shapes.LShape[i][j]
      //     );
      //     if (me.shapes.LShape[i][j]) {
      //       count++;
      //       console.log("count cell=" + count);
      //       console.log("get active cell " + cellRow + "-" + cellCol);
      //       let tempCell = document.getElementById(cellRow + "-" + cellCol);
      //       //debugger
      //       if (tempCell) tempCell.classList.add("activeCell");
      //     }
      //   }
      // }
      // console.log("==================================================");
      let me = this;
      new Promise(resolve => {
        me.cleanActiveCell();
        resolve(true);
      }).then(() => {
        me.renderShape(row, col);
      });
    },
    cleanActiveCell() {
      //clean previous shape
      var cells = document.getElementsByClassName("boardCell activeCell");
      cells.forEach(cell => {
        cell.classList.remove("activeCell");
      });
    },

    renderShape(row, col) {
      //render shape
      var count = 1;
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          let cellRow = row + i;
          let cellCol = col + j;
          console.log(
            "cell " + cellRow + "-" + cellCol + "= " + this.shapes.LShape[i][j]
          );
          if (this.shapes.OSquare[i][j]) {
           
            console.log("count cell=" + count);
            console.log("get active cell " + cellRow + "-" + cellCol);
            let tempCell = document.getElementById(cellRow + "-" + cellCol);
            //debugger
            if (tempCell) tempCell.classList.add("activeCell");
             count++;
          }
        }
      }
      console.log("==================================================");
      
    },

    runGame() {
      let me = this;
      setInterval(function() {
        new Promise(resolve => {
          me.drawShape(me.dropRow, 0, 1);
          resolve();
        }).then(() => {
          me.dropShape();
        });
      }, 3000);
    }
  }
};
</script>

<style>
#gameBoard {
  border: 2px solid black;
  width: 310px;
  height: 620px;
}

.boardCell {
  border: 0.5px solid rgb(224, 224, 224);
  width: 30px;
  height: 30px;
  display: box;
  float: left;
  /* Firefox */
  display: -moz-box;
  /* Safari and Chrome */
  display: -webkit-box;
  background: gray;
}
.activeCell {
  background-image: url("../assets/images/one-punch-man.png");
  background-repeat: no-repeat;
  background-position: 50% 50%;
  background-size: cover;
}
</style>
