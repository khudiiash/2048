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
    transition: Boolean
  },
  mounted() {
    if (this.transition) {
      this.$refs.thisTile.style.top = `${this.from.top}px`;
      this.$refs.thisTile.style.left = `${this.from.left}px`;
      setTimeout(() => {
        this.$refs.thisTile.style.top = `${this.to.top}px`
        this.$refs.thisTile.style.left = `${this.to.left}px`
      }, 0);
    } else {
      this.$refs.thisTile.style.opacity = 0;
      setTimeout(() => (this.$refs.thisTile.style.opacity = `1`), 200);
    }
    if (this.tile.spawn) {
      this.$refs.thisTile.style.transition = "200ms"
      setTimeout(()=> {
        this.$refs.thisTile.style.transform = "scale(1)";
      },200)
      
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
  -webkit-transition: left 200ms ease-in-out, top 200ms ease-in-out;
  transition: left 200ms ease-in-out, top 200ms ease-in-out;
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