<template>
  <div
    class="tile"
    ref="thisTile"
    :style="
      position && {
        left: position.left + 'px',
        top: position.top + 'px',
      }
    "
    :num="num"
  >
    {{ num }}
  </div>
</template>

<script>
import gsap from 'gsap'
export default {
  name: "Tile",
  props: {
    num: Number,
    coor: Number,
    cellXY: Number,
    position: Object,
    from: Object,
    to: Object,
    tile: Object,
    transition: Boolean,
    type: String
  },
  methods: {
    move() {
      if (!this.$refs.thisTile||!this.to) return
      const {thisTile} = this.$refs
      const toTile = Array.from(document.querySelectorAll('.tile')).find(t => t.offsetLeft === Math.round(this.to.left) && t.offsetTop === Math.round(this.to.top))
      const clone = thisTile.cloneNode(true);
      gsap.set(toTile, {opacity: 0})
      gsap.set(clone, {left: this.from.left, top: this.from.top})
      document.querySelector('#app').appendChild(clone)
    
      gsap.to(clone, 0.2, {left: this.to.left, top: this.to.top,
                  onComplete: () => {
                    if (toTile) gsap.set(toTile, {opacity: 1})
                    clone.remove()
                  }})
    }
  },
  mounted() {
    if (this.transition || this.from || this.tile.merged) {
      gsap.from(this.$refs.thisTile, 0, {opacity: 0, delay: .2})
      this.move();
    } else {
        if (!this.$refs.thisTile) return
      }
    if (this.tile.spawn) {
       gsap.from(this.$refs.thisTile, .2, {scale: 0})
    }
  }
};
</script>

<style lang="scss">
.tile {
  position: absolute;
  background: rgb(206, 206, 206);
  color: rgb(112, 112, 112);
  width: 90px;
  height: 90px;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 5px;
  background: #eee4da;
  text-align: center;
  font-weight: bold;
  font-size: 55px;
  // -webkit-transition: left 200ms ease-in-out, top 200ms ease-in-out;
  // transition: left 200ms ease-in-out, top 200ms ease-in-out;
  &[num="4"] {
    background: #ede0c8;
  }
  &[num="8"] {
    color: #f9f6f2;
    background: #f2b179;
  }
  &[num="16"] {
    color: #f9f6f2;
    background: #f59563;
  }
  &[num="32"] {
    color: #f9f6f2;
    background: #f67c5f;
  }
  &[num="64"] {
    color: #f9f6f2;
    background: #f65e3b;
  }
  &[num="128"] {
    color: #f9f6f2;
    background: #edcf72;
    box-shadow: 0 0 30px 10px rgba(243, 215, 116, 0.2381), inset 0 0 0 1px rgba(255, 255, 255, 0.14286);
    font-size: 45px;
  }
  &[num="256"] {
    color: #f9f6f2;
    background: #edcc61;
    box-shadow: 0 0 30px 10px rgba(243, 215, 116, 0.31746), inset 0 0 0 1px rgba(255, 255, 255, 0.19048);
    font-size: 45px;
  }
  &[num="512"] {
    color: #f9f6f2;
    background: #edc850;
    box-shadow: 0 0 30px 10px rgba(243, 215, 116, 0.39683), inset 0 0 0 1px rgba(255, 255, 255, 0.2381);
    font-size: 45px;
  }
  &[num="1024"] {
    color: #f9f6f2;
    background: #edc53f;
    box-shadow: 0 0 30px 10px rgba(243, 215, 116, 0.47619), inset 0 0 0 1px rgba(255, 255, 255, 0.28571);
    font-size: 35px;
  }
  &[num="2048"] {
    color: #f9f6f2;
    background: #edc22e;
    box-shadow: 0 0 30px 10px rgba(243, 215, 116, 0.55556), inset 0 0 0 1px rgba(255, 255, 255, 0.33333);
    font-size: 35px;
  }
}
</style>