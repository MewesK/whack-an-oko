<script lang="ts">
import { defineComponent, PropType } from "vue";

export default defineComponent({
  data() {
    return {
      hit: false,
    };
  },
  methods: {
    onClick() {
      this.hit = true;
      this.$emit('bonk');
    }
  }
});
</script>

<template>
  <div class="mole" @click="onClick">
    <img class="hole-sprite" src="../assets/oko-hole.png" />
    <transition-group name="peek">
      <img v-if="!hit" class="mole-sprite" src="../assets/oko-bonk.png" />
    </transition-group>
    <img class="shadow-sprite" src="../assets/oko-shadow.png" />
    <img class="overlay-sprite" src="../assets/oko-overlay.png" />
  </div>
</template>

<style scoped>
.mole {
  cursor: pointer;
  position: relative;
  width: 72px;
  height: 67px;
}

.mole-sprite {
  position: absolute;
  top: 0;
  left: 8px;
}

.hole-sprite, .shadow-sprite {
  position: absolute;
  bottom: 0;
}

.overlay-sprite {
  position: absolute;
  top: 60px;
}

/* Transition */

.peek-enter-active,
.peek-leave-active {
  transition: all 0.1s linear;
}

.peek-enter-from,
.peek-leave-to {
  top: 67px;
}
</style>
