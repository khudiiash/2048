<template>
  <div v-if="occupied" :xy="xy" class="cell">
    <Tile
      :cellXY="xy"
      :position="position"
      :from="occupied.from"
      :to="occupied.to"
      :num="occupied.num"
      :coor="occupied.coor"
      :tile="occupied"
    />
  </div>
  <div v-else-if="this.old" class="cell">
    <div>
      <Tile
        :cellXY="xy"
        :position="position"
        :from="old.from"
        :to="old.to"
        :num="old.num"
        :coor="old.coor"
        :tile="old"
        :transition="true"
      />
    </div>
  </div>
  <div v-else class="cell"></div>
</template>

<script>
import Tile from "./Tile";
export default {
  name: "Cell",
  data() {
    return {
      old: null,
    }
  },

  components: {
    Tile
  },
  props: {
    xy: Number,
    occupied: Object,
    position: Object
  },
  watch: {
    occupied: function(tile, old) {
      if (tile === null && old !== null) {
        this.old = old
        setTimeout(() => this.old = null, 200)
      }
    }
  }
  
};
</script>

<style>
.cell {
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 25px;
  border-radius: 3px;
  background: rgba(238, 228, 218, 0.35);
  margin: 5px;
}
</style>
