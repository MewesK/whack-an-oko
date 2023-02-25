<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
  props: {
    active: {
      type: Boolean,
      required: true,
    },
  },
  data() {
    return {
      hit: true,
      timeoutId: 0,
    };
  },
  mounted() {
    this.activate();
  },
  watch: {
    active(newValue: Boolean, oldValue: Boolean) { 
      if (!oldValue && newValue) { 
        this.activate();
      }
      if (oldValue && !newValue) {
        this.hit = true;
      }
    }
  },
  methods: {
    activate() {
      if (!this.active) { 
        return;
      }

      setTimeout(() => {
        if (!this.active) { 
          return;
        }

        this.hit = false;
        this.timeoutId = setTimeout(() => {
          if (!this.active) { 
            return;
          }

          if (!this.hit) {
            this.hit = true;
            this.$emit('missed');
          }
        }, (Math.random() * 1.2 + 0.8) * 1000);
      }, (Math.random() * 5 + 1) * 1000)
    },
    onClick() {
      if (!this.active) { 
        return;
      }

      if (!this.hit) {
        this.hit = true;
        clearTimeout(this.timeoutId);
        this.$emit('bonk');
        this.activate();
      }
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
