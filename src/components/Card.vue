<template>
  <div
    class="cardd"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${(600 - 16 * 4) / Math.sqrt(cardContext.length) - 16}px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) / 4
      }px`,
    }"
  >
    <div
      class="card__inner"
      :class="{ is__flipped: isFlip }"
      @click="onToggleFlipCard"
    >
      <div class="card__face card__face--front">
        <div
          class="card__content"
          :style="{
            backgroundSize: `${
              (((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              3
            }px ${
              (((920 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3) /
              4 /
              3
            }px`,
          }"
        ></div>
      </div>
      <div class="card__face card__face--back">
        <div
          class="card__content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<style lang="css">
.cardd {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card__inner {
  width: 100%;
  height: 100%;
  transition: transform 1s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.cardd.disabled .card__inner {
  /* cursor: default; */
  display: none;
  transition: all linear 1s;
  opacity: 0;
}

.card__inner.is__flipped {
  transform: rotateY(-180deg);
}

.card__face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}

.card__face--front .card__content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  background-size: 60px 60px;
  width: 100%;
  height: 100%;
}

.card__face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card__face--back .card__content {
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
}
</style>

<script>
export default {
  props: {
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    card: {
      type: [String, Number, Array, Object],
    },
    cardContext: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isFlip: false,
      isDisabled: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled) return false;
      this.isFlip = !this.isFlip;
      if (this.isFlip) this.$emit("onFlip", this.card);
    },
    onFlipBack() {
      this.isFlip = false;
    },
    onEnableMode() {
      this.isDisabled = true;
    },
  },
};
</script>