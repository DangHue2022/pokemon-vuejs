<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${
        (this.availHeight - 72 - 16 * 4) / Math.sqrt(cardContexts.length) - 16
      }px`,
      width: `${
        (((this.availHeight - 72 - 16 * 4) / Math.sqrt(cardContexts.length) -
          16) *
          3) /
        4
      }px`,
      perspective: `${
        ((((this.availHeight - 72 - 16 * 4) / Math.sqrt(cardContexts.length) -
          16) *
          3) /
          4) *
        2
      }px`,
    }"
  >
    <div
      class="card_inner"
      :class="{ is_flipped: isFlipped }"
      @click="onTongleFlipCard"
    >
      <div class="card_face card_face--front">
        <div
          class="card_content"
          :style="{
            backgroundSize: `${
              (((this.availHeight - 72 - 16 * 4) /
                Math.sqrt(cardContexts.length) -
                16) *
                3) /
              4 /
              3
            }px ${
              ((this.availHeight - 72 - 16 * 4) /
                Math.sqrt(cardContexts.length) -
                16) /
              4
            }px`,
          }"
        ></div>
      </div>
      <div class="card_face card_face--back">
        <div
          class="card_content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imageUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imageUrl: {
      type: String,
      required: true,
    },
    card: {
      type: [String, Number, Array, Object],
    },
    cardContexts: {
      type: Array,
      default: function () {
        return [];
      },
    },
  },
  data() {
    return {
      isDisabled: false,
      isFlipped: false,
      availWidth: screen.availWidth,
      availHeight: screen.availHeight,
    };
  },
  methods: {
    onTongleFlipCard() {
      if (this.isDisabled) return false;
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) {
        this.$emit("onFlip", this.card);
      }
    },
    onFlipCardBack() {
      this.isFlipped = false;
    },
    onEnalBleDisabled() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 1rem;
  margin-bottom: 1rem;
}

.card.disabled .card_inner {
  cursor: default;
}

.card_inner {
  width: 100%;
  height: 100%;
  cursor: pointer;
  transition: transform 1s;
  transform-style: preserve-3d;
  position: relative;
}

.card_inner.is_flipped {
  transform: rotateY(-180deg);
}

.card_face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  padding: 1rem;
  box-shadow: 0 3px 5px 3px rgba(0, 0, 0, 0.1);
}

.card_face--back {
  background-color: var(--light);
  transform: rotateY(-180deg);
}

.card_face--back .card_content {
  background-position: center center;
  background-repeat: no-repeat;
  background-size: contain;
  width: 100%;
  height: 100%;
}

.card_face--front .card_content {
  background: url("../assets/images/icon_back.png") no-repeat center center;
  width: 100%;
  height: 100%;
}
</style>
