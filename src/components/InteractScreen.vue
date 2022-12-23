<template>
  <div class="screen">
    <div
      class="screen__inner"
      :style="{
        width: `${
          ((((990 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4 +
          16) * Math.sqrt(cardContext.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        @onFlip="checkRule($event)"
        :cardContext="cardContext"
      />
    </div>
  </div>
</template>

<script>
import CardFlip from "./Card.vue";

export default {
  props: {
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  components: {
    CardFlip,
  },
  data() {
    return {
      rules: [],
    };
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false;

      this.rules.push(card);

      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        setTimeout(() => {
          // add 'disabled'
          this.$refs[`card-${this.rules[0].index}`][0].onEnableMode();
          this.$refs[`card-${this.rules[1].index}`][0].onEnableMode();
          // reset rules
          this.rules = [];
        }, 800);
        const disabledElements = document.querySelectorAll(
          ".screen .cardd.disabled"
        );
        if (
          disabledElements &&
          disabledElements.length === this.cardContext.length - 2
        ) {
          setTimeout(() => {
            this.$emit("onFinished");
          }, 1000);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          // close card
          this.$refs[`card-${this.rules[0].index}`][0].onFlipBack();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipBack();
          // reset rules
          this.rules = [];
        }, 800);
      } else return false;
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100%;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
}

.screen__inner {
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>

