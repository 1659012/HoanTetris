<template>
  <div id="gameBoard" ref="gameBoard">
    <h3>{{dropCol}}</h3>
    <h3>{{dropRow}}</h3>
  </div>
</template>

<script>
import _ from "lodash";
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
      dropCol: 3,
      shapeTypes: [
        "IShape", //0
        "JShape", //1
        "LShape", //2
        "OShape", //3
        "SShape", //4
        "TShape", //5
        "ZShape" //6
      ],
      shapeTypeIndex: 0, //type I,J, O,...
      shapeGenericIndex: 0, //type I0, I1,...
      currentShape: null,
      shapes: {
        IShape: [[1, 0, 0], [1, 0, 0], [1, 0, 0]],

        JShape: [[0, 1, 0], [0, 1, 0], [1, 1, 0]],

        LShape: [[0, 1, 0], [0, 1, 0], [0, 1, 1]],

        OShape: [[1, 1, 0], [1, 1, 0], [0, 0, 0]],
        SShape: [[0, 1, 1], [1, 1, 0], [0, 0, 0]],
        TShape: [[1, 1, 1], [0, 1, 0], [0, 1, 0]],
        ZShape: [[1, 1, 0], [0, 1, 1], [0, 0, 0]]
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
      let me = this;
      new Promise(resolve => {
        me.createNewBoard();
        resolve();
      }).then(() => {
        me.runGame();
        me.listenToKeyDown();
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

    listenToKeyDown() {
      let me = this;
      document.onkeydown = function(e) {
        switch (e.keyCode) {
          case 32: //space
            //console.log("space");
            break;

          case 37: //left
            //console.log("left");
            if (me.dropCol > 0) me.dropCol--;
            me.clearActiveCell();
            break;

          case 39: //right
            //console.log("right");
            if (me.dropCol < me.maxCol) me.dropCol++;
            me.clearActiveCell();
            break;

          case 38: //up
            console.log("up");
            break;

          case 40: //down
            console.log("down");
            break;
        }
      };
    },

    dropShape() {
      if (this.dropRow < this.maxRow) {
        this.dropRow++;
      } 
    },

    drawShape(row, col, shapeTypeIndex, shapeGenericIndex) {
      let me = this;
      //clean previous shape
      var cells = document.getElementsByClassName("activeCell");
      cells.forEach(cell => {
        cell.classList.remove("activeCell");
      });

      //render shape
      var currentShape = [];
      switch (shapeTypeIndex) {
        case 0: //IShape
          currentShape = me.shapes.IShape;
          break;

        case 1: //J
          currentShape = me.shapes.JShape;
          break;

        case 2: //L
          currentShape = me.shapes.LShape;
          break;

        case 3: //O
          currentShape = me.shapes.OShape;
          break;

        case 4: //S
          currentShape = me.shapes.SShape;
          break;

        case 5: //T
          currentShape = me.shapes.TShape;
          break;

        case 6: //Z
          currentShape = me.shapes.ZShape;
          break;

        default:
          currentShape = me.shapes.IShape;
          break;
      }

      //debugger
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          let cellRow = row + i;
          let cellCol = col + j;
          if (currentShape[i][j]) {
            //console.log("get active cell " + cellRow + "-" + cellCol);
            let tempCell = document.getElementById(cellRow + "-" + cellCol);
            //debugger
            if (tempCell) tempCell.classList.add("activeCell");
          }
        }
      }
      //console.log("==================================================");
    },

    clearActiveCell() {
      var cells = document.getElementsByClassName("boardCell activeCell");
      cells.forEach(cell => {
        cell.classList.remove("activeCell");
      });
    },

    getRandomShape() {
      this.dropRow = 0;
      this.dropCol = 3; //format shape to drop center     
      var shapeLength = this.shapeTypes.length;
      this.shapeTypeIndex = Math.floor(Math.random() * shapeLength);
      console.log("shape type"+ this.shapeTypeIndex);
    },

    runGame() {
      let me = this;
      //setInterval
      // setInterval(function() {
      //   new Promise(resolve => {
      //     me.drawShape(
      //       me.dropRow,
      //       me.dropCol,
      //       me.shapeTypeIndex,
      //       me.shapeGenericIndex
      //     );
      //     resolve();
      //   }).then(() => {
      //     if (me.dropRow < me.maxRow - 3) {            
      //       me.dropShape();
      //     } else {
      //       me.getRandomShape();
      //     }
      //   });
      // }, 500);

       setInterval(function() {
        new Promise(resolve => {
          me.drawShape(
            me.dropRow,
            me.dropCol,
            me.shapeTypeIndex,
            me.shapeGenericIndex
          );
          resolve();
        }).then(() => {
          me.getRandomShape();
        });
      }, 500);
    }
  }
};
</script>

<style>
#gameBoard {
  border: 2px solid black;
  width: 310px;
  height: 620px;
  z-index: 99999;
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
  /* background-image: url("../assets/images/one-punch-man.png");
  background-repeat: no-repeat;
  background-position: 50% 50%;
  background-size: cover; */
  background-color: blue;
}
</style>
