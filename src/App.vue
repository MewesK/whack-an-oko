<script lang="ts">
import { defineComponent } from "vue";

import NumericDisplay from "./components/NumericDisplay.vue";
import Mole from "./components/Mole.vue";
import TextBox from "./components/TextBox.vue";

export default defineComponent({
  components: { Mole, NumericDisplay, TextBox },
  data() {
    return {
      // Options
      scale: 1,
      smooth: false,
      fullPixel: false,
      // Current game
      missed: 3,
      score: 0,
      message: [] as Array<string>,
    };
  },
  mounted() {
    this.onResize();
    window.addEventListener("resize", this.onResize);
    this.message = [
      'Get Ready!\nClick This Box To Start!'
    ];
  },
  unmounted() {
    window.removeEventListener("resize", this.onResize);
  },
  methods: {
    onBonk() {
      this.score++;
    },
    onMissed() {
      this.missed++;
      if (this.missed >= 3) { 
        this.message = [
          'You Lost!\nBut Happy 1 Year Twitch Annivesary, Oko!',
          'Thanks for being such a sweetheart <3\n-MewK'
        ];
      }
    },
    onClose() {
      this.message = [];
      this.missed = 0;
      this.score = 0;
    },
    onResize() {
      this.scale = Math.max(
        this.fullPixel
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
    },
  },
});
</script>

<template>
  <div
    id="field"
    :class="{ pixelated: fullPixel || !smooth }"
    :style="{ transform: `scale(${scale})` }"
    @contextmenu.prevent
  >
    <img id="logo" src="./assets/oko-logo.png">
    <div id="stats">
      <div id="number-missed">
        &nbsp;Missed:<br>
        <numeric-display
          :value="missed"
          :digits="3"
        />
        of
        <numeric-display
          :value="3"
          :digits="3"
        />
      </div>
      <div id="number-score">
        &nbsp;Score:<br>
        <numeric-display
          :value="score"
          :digits="3"
        />
      </div>
    </div>
    <div id="moles">
      <mole :active="missed < 3" @bonk="onBonk" @missed="onMissed" v-for="index in 5" :key="index" />
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

#logo {
  position: absolute;
  top: 4px;
  left: 25px;
}

#stats {
  position: absolute;
  top: 160px;
  left: 10px;
}

#stats > div, #stats * {
  position: relative;
  display: inline-block;
}

#number-missed {
  left: 20px;
}

#number-score {
  left: 60px;
}

#moles {
  position: absolute;
  top: 160px;

  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  align-content: center;
  gap: 10px;
  margin-top: 40px;

  overflow: hidden;
}
</style>