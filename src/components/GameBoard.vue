<template>
  <div id="gameBoard" ref="gameBoard"></div>
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
      dropCol: 3,
      shapeTypes: [
        "IShape",
        "JShape",
        "LShape",
        "OShape",
        "SShape",
        "TShape",
        "ZShape"
      ],
      shapes: {
        IShape: [
          [1, 0],
          [3, 1],
          [2, 3],
          [0, 2]
        ],
        JShape: [
          [1, 0],
          [2, 1],
          [1, 2],
          [2, 0]
        ],

        LShape: [
          [0, 1, 0],
          [0, 1, 0],
          [0, 1, 1]
        ],

        OShape: [
          [1, 1, 0],
          [1, 1, 0],
          [0, 0, 0]
        ],
        SShape: [
          [0, 2],
          [0, 0],
          [2, 0],
          [2, 2]
        ],
        TShape: [
          [0, 1],
          [1, 0],
          [2, 1],
          [1, 2]
        ],
        ZShape: [
          [0, 1],
          [1, 0],
          [2, 1],
          [1, 2]
        ]
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
        this.listenToKeyDown();
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
      } else if (this.dropRow >= this.maxRow) {
        this.dropRow = 0;
        this.drowCol = 3; //format shape to drop center
      }
    },

    drawShape(row, col, shapeType, shapeIndex) {
      //clean previous shape
      var cells = document.getElementsByClassName("boardCell activeCell");
      cells.forEach(cell => {
        cell.classList.remove("activeCell");
      });

      //render shape
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          let cellRow = row + i;
          let cellCol = col + j;
          //console.log("cell " + cellRow + "-" + cellCol + "= " + this.shapes.LShape[i][j]);
          if (this.shapes.LShape[i][j]) {
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

    runGame() {
      let me = this;
      setInterval(function() {
        new Promise(resolve => {
          me.drawShape(me.dropRow, me.dropCol, 1, 1);
          resolve();
        }).then(() => {
          me.dropShape();
          if (me.dropRow >= me.maxRow) {
            me.dropRow = 0;
            me.drowCol = 3; //format shape to drop center
          }
        });
      }, 1000);
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
