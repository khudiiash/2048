<template>
  <div class="game" id="game">
    <div>
      <h1 class="title">2048</h1>
    </div>
    <div class="board">
      <Cell
        pos="11"
        :xy="11"
        :position="this.coords[11]"
        :occupied="this.cells[11]"
      />
      <Cell
        :xy="12"
        pos="12"
        :position="this.coords[12]"
        :occupied="this.cells[12]"
      />
      <Cell
        :xy="13"
        pos="13"
        :position="this.coords[13]"
        :occupied="this.cells[13]"
      />
      <Cell
        :xy="14"
        pos="14"
        :position="this.coords[14]"
        :occupied="this.cells[14]"
      />
      <Cell
        :xy="21"
        pos="21"
        :position="this.coords[21]"
        :occupied="this.cells[21]"
      />
      <Cell
        :xy="22"
        pos="22"
        :position="this.coords[22]"
        :occupied="this.cells[22]"
      />
      <Cell
        :xy="23"
        pos="23"
        :position="this.coords[23]"
        :occupied="this.cells[23]"
      />
      <Cell
        :xy="24"
        pos="24"
        :position="this.coords[24]"
        :occupied="this.cells[24]"
      />
      <Cell
        :xy="31"
        pos="31"
        :position="this.coords[31]"
        :occupied="this.cells[31]"
      />
      <Cell
        :xy="32"
        pos="32"
        :position="this.coords[32]"
        :occupied="this.cells[32]"
      />
      <Cell
        :xy="33"
        pos="33"
        :position="this.coords[33]"
        :occupied="this.cells[33]"
      />
      <Cell
        :xy="34"
        pos="34"
        :position="this.coords[34]"
        :occupied="this.cells[34]"
      />
      <Cell
        :xy="41"
        pos="41"
        :position="this.coords[41]"
        :occupied="this.cells[41]"
      />
      <Cell
        :xy="42"
        pos="42"
        :position="this.coords[42]"
        :occupied="this.cells[42]"
      />
      <Cell
        :xy="43"
        pos="43"
        :position="this.coords[43]"
        :occupied="this.cells[43]"
      />
      <Cell
        :xy="44"
        pos="44"
        :position="this.coords[44]"
        :occupied="this.cells[44]"
      />
    </div>
  </div>
</template>

<script>
import Cell from "./Cell";

const reversed = (a, b) => {
  // for reversing cols and rows
  // input [Tile, Tile, null, null]
  // output [null, null, Tile, Tile]
  var va = a === null ? "" : "" + a,
    vb = b === null ? "" : "" + b;
  return va > vb ? 1 : va === vb ? 0 : -1;
};
const getRandomCell = () => {
  // get random cell coordinate
  let min = Math.ceil(1);
  let max = Math.floor(4);
  let row = Math.floor(Math.random() * (max - min + 1)) + min;
  let col = Math.floor(Math.random() * (max - min + 1)) + min;
  let randCell = parseInt(`${row}${col}`);
  return Math.round(randCell);
};
const getRandomTileNumber = () => {
  // get random number for tile (2 || 4)
  return Math.random() < 0.5 ? 2 : 4;
};
class Tile {
  constructor(num, coor) {
    this.coor = coor;
    this.num = num;
    this.from;
    this.to;
    this.spawn = true;
    this.merged = false;
  }
  merge({ from, to }) {
    this.from = from;
    this.to = to;
    this.num = this.num + this.num;
    this.merged = true;
    this.spawn = false;
    return this;
  }
  move({ cellXY, from, to }) {
    this.from = from;
    this.to = to;
    this.coor = cellXY;
    this.spawn = false;
    return this;
  }
  row() {
    return parseInt(this.coor.toString().charAt(0));
  }
  col() {
    return parseInt(this.coor.toString().charAt(1));
  }
}
export default {
  components: {
    Cell
  },
  name: "Game",
  data() {
    return {
      score: 0,
      coords: {
        11: null,
        12: null,
        13: null,
        14: null,
        21: null,
        22: null,
        23: null,
        24: null,
        31: null,
        32: null,
        33: null,
        34: null,
        41: null,
        42: null,
        43: null,
        44: null
      },
      cells: {
        11: null,
        12: null,
        13: null,
        14: null,
        21: null,
        22: null,
        23: null,
        24: null,
        31: null,
        32: null,
        33: null,
        34: null,
        41: null,
        42: null,
        43: null,
        44: null
      }
    };
  },
  methods: {
    arrange(dir) {
      let col1 = [],
        col2 = [],
        col3 = [],
        col4 = [],
        row1 = [],
        row2 = [],
        row3 = [],
        row4 = [];

      for (let [key] of Object.entries(this.cells)) {
        switch (key.charAt(0)) {
          case "1":
            row1.push(this.cells[key]);
            break;
          case "2":
            row2.push(this.cells[key]);
            break;
          case "3":
            row3.push(this.cells[key]);
            break;
          case "4":
            row4.push(this.cells[key]);
            break;
          default:
            break;
        }
        switch (key.charAt(1)) {
          case "1":
            col1.push(this.cells[key]);
            break;
          case "2":
            col2.push(this.cells[key]);
            break;
          case "3":
            col3.push(this.cells[key]);
            break;
          case "4":
            col4.push(this.cells[key]);
            break;
          default:
            break;
        }
      }
      if (["up", "down"].includes(dir)) {
        [col1, col2, col3, col4].forEach((col, c) => {
          col.sort();
          if (dir === "down") col = col.sort(reversed);
          col.forEach((tile, r) => {
            if (tile) {
              this.cells[`${r + 1}${c + 1}`] = tile.move({
                cellXY: parseInt(`${r + 1}${c + 1}`),
                from: this.coords[tile.coor],
                to: this.coords[parseInt(`${r + 1}${c + 1}`)]
              });
            } else {
              this.cells[`${r + 1}${c + 1}`] = null;
            }
          });
        });
      } else {
        [row1, row2, row3, row4].forEach((row, r) => {
          row.sort();
          if (dir === "right") row = row.sort(reversed);
          row.forEach((tile, c) => {
            if (tile) {
              this.cells[`${r + 1}${c + 1}`] = tile.move({
                cellXY: parseInt(`${r + 1}${c + 1}`),
                from: this.coords[tile.coor],
                to: this.coords[parseInt(`${r + 1}${c + 1}`)]
              });
            } else {
              this.cells[`${r + 1}${c + 1}`] = null;
            }
          });
        });
      }
    },
    spawn() {
      // spawn one random tile
      setTimeout(() => {
        let occupied = false;
        let isFree = Object.values(this.cells).some(i => i === null)
        if (isFree) {
          while (!occupied) {
            let cell = getRandomCell();
            let num = getRandomTileNumber();
            let allFree = Object.values(this.cells).every(i => i === null)
            if (!this.cells[cell]) {
              this.cells[cell] = new Tile(num, cell);
              occupied = allFree ? false : true;
            }
          }
        }
      }, 50);
     
    },
    move(dir) {
      this.arrange(dir);
      ["1", "2", "3", "4"].forEach(i => {
        let tiles = ["up", "down"].includes(dir)
          ? [
              this.cells[`1${i}`],
              this.cells[`2${i}`],
              this.cells[`3${i}`],
              this.cells[`4${i}`]
            ].filter(c => c !== null)
          : [
              this.cells[`${i}1`],
              this.cells[`${i}2`],
              this.cells[`${i}3`],
              this.cells[`${i}4`]
            ].filter(c => c !== null);
        if (tiles.length) {
          tiles.reduce((prev, cur) => {
            if (!prev) return cur;
            else {
              if (prev.num === cur.num) {
                this.score += prev.num
                this.cells[prev.coor] = cur.merge({
                  cellXY: this.coords[prev.coor],
                  from: this.coords[cur.coor],
                  to: this.coords[prev.coor]
                });
                this.cells[cur.coor] = null;
                this.arrange(dir);
                return tiles.length % 2 === 0 ? cur : prev;
              }
              return cur;
            }
          });
        }
      });
      this.spawn()
    }
  },
  mounted() {
    let cells = this.$el.querySelectorAll(".cell");
    for (let cell of cells) {
      if (cell) {
        let { left, top } = cell.getBoundingClientRect();
        this.coords[cell.attributes.pos.value] = { left: left, top: top };
      }
    }

    var touchstartX = 0;
    var touchstartY = 0;
    var touchendX = 0;
    var touchendY = 0;

    var gesuredZone = document.body;

    gesuredZone.addEventListener(
      "touchstart",
      event => {
        touchstartX = event.targetTouches[0].screenX;
        touchstartY = event.targetTouches[0].screenY;
      },
      false
    );

    gesuredZone.addEventListener(
      "touchend",
      event => {
        touchendX = event.changedTouches[0].screenX;
        touchendY = event.changedTouches[0].screenY;
        (() => {
          if (
            Math.abs(touchendX - touchstartX) >
            Math.abs(touchendY - touchstartY)
          ) {
            if (touchendX < touchstartX) {
              this.move("left");
            } else if (touchendX > touchstartX) {
              this.move("right");
            }
          } else {
            if (touchendY < touchstartY) {
              this.move("up");
            } else if (touchendY > touchstartY) {
              this.move("down");
            }
          }
        })();
      },
      false
    );

    window.addEventListener("keyup", e => {
      switch (e.key) {
        case "ArrowUp":
          this.move("up");
          break;
        case "ArrowDown":
          this.move("down");
          break;
        case "ArrowLeft":
          this.move("left");
          break;
        case "ArrowRight":
          this.move("right");
          break;
      }
    });
    this.spawn();
  }
};
</script>

<style lang="scss">
.title {
  font-size: 80px;
  font-weight: bold;
  margin-right: 220px;
  margin-bottom: 0;
  box-sizing: border-box;
  color: #776e65;
}
.score {
  font-size: 30px;
  font-weight: bold;
  margin-right: 220px;
  margin-bottom: 0;
  box-sizing: border-box;
  color: #776e65;
}
.board {
  width: 400px;
  box-sizing: border-box;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: minmax(100px, auto);
  background: #bbada0;
  padding: 5px;
  border-radius: 5px;
}
</style>
