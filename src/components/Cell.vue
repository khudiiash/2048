<template>
  <div v-if="occupied && !fresh" :xy="xy" class="cell">
    <Tile
      :cellXY="xy"
      :position="position"
      :from="occupied.from"
      :to="occupied.to"
      :num="occupied.num"
      :coor="occupied.coor"
      :tile="occupied"
      :type="'new'"
    />
  </div>
  <div v-else-if="this.old && !this.fresh" class="cell">
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
        :type="'old'"
      />
    </div>
  </div>
  <div v-else-if="!this.old && this.fresh" class="cell">
    <div>
      <Tile
        :cellXY="xy"
        :position="position"
        :from="fresh.from"
        :to="fresh.to"
        :num="fresh.num"
        :coor="fresh.coor"
        :tile="fresh"
        :transition="true"
        :type="'old'"
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
      fresh: null
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
      if (!tile && old) {
        this.old = old;
        setTimeout(() => (this.old = null), 300);
      } else if (tile && old) {
        this.fresh = tile;
        this.old = old;
        setTimeout(() => (this.fresh = null), 300);
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
