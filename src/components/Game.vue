<template>
  <div class="game" id="game">
     <div class='score'>Score<div>{{score}}</div></div>
     <div class='best'>Best<div>{{best || score}}</div></div>
     <div class='replay' @click="restart">
      <svg version="1.1" id="Capa_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
            viewBox="0 0 76.398 76.398" xml:space="preserve">
            <path d="M58.828,16.208l-3.686,4.735c7.944,6.182,11.908,16.191,10.345,26.123C63.121,62.112,48.954,72.432,33.908,70.06
              C18.863,67.69,8.547,53.522,10.912,38.477c1.146-7.289,5.063-13.694,11.028-18.037c5.207-3.79,11.433-5.613,17.776-5.252
              l-5.187,5.442l3.848,3.671l8.188-8.596l0.002,0.003l3.668-3.852L46.39,8.188l-0.002,0.001L37.795,0l-3.671,3.852l5.6,5.334
              c-7.613-0.36-15.065,1.853-21.316,6.403c-7.26,5.286-12.027,13.083-13.423,21.956c-2.879,18.313,9.676,35.558,27.989,38.442
              c1.763,0.277,3.514,0.411,5.245,0.411c16.254-0.001,30.591-11.85,33.195-28.4C73.317,35.911,68.494,23.73,58.828,16.208z"/>
      </svg>
     </div>
     
    <div class='header'>
      <h1 class="title">2048</h1>
    </div>
    <div class="board">
      <Cell
        v-for="n of board"
        :key="n"
        :pos="n"
        :xy="n"
        :position="coords[n]"
        :occupied="cells[n]"
      />
    </div>
  </div>
</template>

<script>
import Cell from "./Cell";
import gsap from 'gsap'
console.log(gsap)
function getDefaultData() {
    const board = [11,12,13,14,
                  21,22,23,24,
                  31,32,33,34,
                  41,42,43,44];
    const coords = {}
    const cells = {}
    for (let n of board) {
      coords[n] = null
      cells[n] = null
    }
    return {
      score: 0,
      best: null,
      board, 
      coords, 
      cells,
      gameOver: true
    };
}

const reversed = (a, b) => {
  // for reversing cols and rows
  // input [Tile, Tile, null, null]
  // output [null, null, Tile, Tile]
  const va = a === null ? "" : "" + a,
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
  data: function() { return getDefaultData() },
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
    setCoords() {
      let cells = this.$el.querySelectorAll(".cell");
      for (let cell of cells) {
        if (cell) {
          let { left, top } = cell.getBoundingClientRect();
          this.coords[cell.attributes.pos.value] = { left: left, top: top };
        }
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
      const stamp = {...this.cells}
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
                this.score += prev.num;
                if (this.best < this.score) this.best = this.score
                // const staticTile = Array.from(document.querySelectorAll('.tile')).find(t => t.parentElement.getAttribute('xy') === String(prev.coor))
                // if (staticTile) {
                //   console.log({staticTile})
                //   const clone = staticTile.cloneNode(true);
                //   clone.classList.add('static')
                //   document.querySelector('#app').appendChild(clone)
                //   gsap.set(clone, {left: this.coords[prev.coor].left, top: this.coords[prev.coor].top})
                //   gsap.to(clone, 0, {autoAlpha: 0, delay: .2, onComplete: () => clone.remove()})
                // }
                this.cells[prev.coor] = cur.merge({
                  cellXY: this.coords[prev.coor],
                  from: this.coords[cur.coor],
                  to: this.coords[prev.coor]
                });
                prev = this.cells[prev.coor];
                this.cells[cur.coor] = null;
                this.arrange(dir);
                return;
              }
              return cur;
            }
          });
        }
      });
      if (JSON.stringify(stamp) === JSON.stringify(this.cells)){
        // no movement, checking if there are free cells
        const isFree = Object.values(this.cells).some(i => i === null)
        // if not, checking for game over
        if (!isFree && this.isGameOver()) this.finish()
      }
      else this.spawn() // tiles were moved, so we spawn a new one
    },
    isGameOver() {
        return !['1', '2', '3', '4'].some((n) => this.cells[n + 1].num / this.cells[n + 2].num === 1 ||
                                                this.cells[n + 2].num / this.cells[n + 3].num === 1 ||
                                                this.cells[n + 3].num / this.cells[n + 4].num === 1 ||
                                                this.cells[1 + n].num / this.cells[2 + n].num === 1 ||
                                                this.cells[2 + n].num / this.cells[3 + n].num === 1 ||
                                                this.cells[3 + n].num / this.cells[4 + n].num === 1)
    },
    finish() {
        console.log('game over')
    },
    restart: function() {
      Object.assign(this._data, getDefaultData(), {best: this._data.score > this._data.best ? this._data.score : this._data.best})
      this.setCoords()
      setTimeout(this.spawn, 500)
    }
  },

  mounted() {
    this.setCoords()
    let touchstartX = 0;
    let touchstartY = 0;
    let touchendX = 0;
    let touchendY = 0;



    window.addEventListener('resize', () => {
        this.setCoords()
    })

    const gesuredZone = document.body;
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
  margin: 0;
  box-sizing: border-box;
  color: #776e65;
}
.header {
  display: flex;
  width: 400px;
}
.score, .best, .replay {
  font-size: 22px;
  width: 120px;
  height: 60px;
  position: absolute;
  left: 10px;
  top: 10px;
  font-weight: bold;
  margin-right: 220px;
  margin: 0;
  border-radius: 4px;
  box-sizing: border-box;
  color: #fff;
  background: #776e65;
}
.best {
   left: 150px;
   top: 10px;
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
.replay {
  width: 60px;
  height: 60px;
  position: absolute;
  left: 300px;
  top: 10px;
  background: #776e65;
  display: flex;
  align-items: center;
  justify-content: center;
}
.replay svg {
  width: 60%;
  height: 60%;
}
path {
  fill: #bbada0;
}

.tile {
  z-index: 1;
}
.tile.static {
  z-index: 0;
}
</style>
