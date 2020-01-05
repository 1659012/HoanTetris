/* eslint-disable */
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
      sizeX: 5,
      sizeY: 5
    };
  },
  mounted() {
    this.initialize();
  },
  computed: {},
  watch: {},
  methods: {
    /* eslint-disable */
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
      for (let x = 0; x < this.sizeX; x++) {
        for (let y = 0; y < this.sizeY; y++) {
          let node = document.createElement("div");
          node.setAttribute("class", "boardCell");
          node.setAttribute("id", x + "-" + y);
          gameBoard.appendChild(node);
        }
      }
    },

    getRadomCell(horizontal, vertical) {
      var x = Math.round(Math.random() * horizontal);
      var y = Math.round(Math.random() * vertical);
      return { x, y };
    },
    chageRamdomCell() {
      let position = this.getRadomCell(this.sizeX - 1, this.sizeY - 1);
      // console.log(position);
      var cell = document.getElementById(position.x + "-" + position.y);
      cell.classList.add("activeCell");

      setTimeout(function() {
        cell.classList.remove("activeCell");
      }, 1000);
    },

    runGame() {
      let me = this;
      setInterval(function() {
        me.chageRamdomCell();
      }, 1200);
    }
  }
};
</script>


<style >
#gameBoard {
  border: 2px solid black;
  width: 510px;
  height: 510px;
}

.boardCell {
  border: 1px solid rgb(224, 224, 224);
  width: 100px;
  height: 100px;
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
