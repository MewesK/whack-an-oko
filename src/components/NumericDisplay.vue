<script lang="ts">
import { defineComponent } from "vue";

import score0 from "../assets/font_score_0.png";
import score1 from "../assets/font_score_1.png";
import score2 from "../assets/font_score_2.png";
import score3 from "../assets/font_score_3.png";
import score4 from "../assets/font_score_4.png";
import score5 from "../assets/font_score_5.png";
import score6 from "../assets/font_score_6.png";
import score7 from "../assets/font_score_7.png";
import score8 from "../assets/font_score_8.png";
import score9 from "../assets/font_score_9.png";

export const DIGITS = [
    score0,
    score1,
    score2,
    score3,
    score4,
    score5,
    score6,
    score7,
    score8,
    score9,
] as Array<string>;

export default defineComponent({
  props: {
    value: {
      type: Number,
      required: true,
    },
    digits: {
      type: Number,
      default: null,
    },
  },
  computed: {
    digitsParsed() {
      return String(this.value)
        .padStart(
          this.digits !== null ? this.digits : Math.ceil(this.value / 10),
          "0"
        )
        .split("")
        .map((digit) => {
          return DIGITS[parseInt(digit)];
        });
    },
  },
});
</script>

<template>
  <div class="numeric-display">
    <img v-for="(path, index) in digitsParsed" :key="index" :src="path" />
  </div>
</template>

<style scoped>
.numeric-display {
  line-height: 0;
}
img {
  margin-left: 1px;
}
</style>
