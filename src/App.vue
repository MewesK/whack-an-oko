<script lang="ts">
import { defineComponent } from "vue";

import NumericDisplay from "./components/NumericDisplay.vue";
import Mole from "./components/Mole.vue";
import TextBox from "./components/TextBox.vue";

function calculateScale(fullPixel: boolean): number {
  return Math.max(
    fullPixel
      ? Math.min(
          Math.floor(document.documentElement.clientHeight / 400),
          Math.floor(document.documentElement.clientWidth / 256)
        )
      : Math.min(
          Math.floor((document.documentElement.clientHeight * 100) / 400) / 100,
          Math.floor((document.documentElement.clientWidth * 100) / 256) / 100
        ),
    1
  );
}

export default defineComponent({
  components: { Mole, NumericDisplay, TextBox },
  data() {
    return {
      // Options
      scale: 1,
      smooth: false,
      fullPixel: false,
      // Current game
      level: 0,
      lives: 3,
      score: 0,
      message: [] as Array<string>,
    };
  },
  created() {
  },
  mounted() {
    this.onResize();
    window.addEventListener("resize", this.onResize);
  },
  unmounted() {
    window.removeEventListener("resize", this.onResize);
  },
  methods: {
    onBonk() {
      this.score++;
    },
    onClose() {
      this.message = [];
    },
    onResize() {
      this.scale = calculateScale(this.fullPixel);
    },
  },
});
</script>

<template>
  <div
    id="field"
    :class="{ pixelated: fullPixel || !smooth }"
    :style="{ transform: `scale(${scale})` }"
  >
    <numeric-display
      id="number-level"
      :value="level + 1"
    />
    <numeric-display
      id="number-lives"
      :value="lives"
    />
    <numeric-display
      id="number-score"
      :value="score"
      :digits="5"
    />
    <div id="moles">
      <mole @bonk="onBonk" v-for="index in 5" :key="index" />
    </div>
    <text-box :pages="message" @close="onClose" />
  </div>
</template>

<style scoped>
#field {
  position: relative;
  width: 256px;
  height: 400px;
  background-color: #d7a58a;
  margin: 0;
  padding: 0;
  text-align: initial;
  transform-origin: center 0;
}

#number-level {

}

#number-lives {

}

#number-score {

}

#moles {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  align-content: center;
  gap: 10px;
  margin-top: 40px;
}
</style>