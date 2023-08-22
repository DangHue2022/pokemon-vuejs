<template>
  <div class="screen">
    <div
      class="screen_inner"
      :style="{
        width: `${
          ((((this.availHeight - 72 - 16 * 4) / Math.sqrt(cardContexts.length) -
            16) *
            3) /
            4 +
            16) *
          Math.sqrt(cardContexts.length)
        }px`,
      }"
    >
      <card-flip
        v-for="(card, index) in cardContexts"
        :key="index"
        :ref="`card-${index}`"
        :imageUrl="`images/${card}.png`"
        :card="{ index, value: card }"
        :cardContexts="cardContexts"
        @onFlip="checkValue($event)"
      />
    </div>
  </div>
</template>

<script>
import cardFlip from "./cardElement.vue";
export default {
  props: {
    cardContexts: {
      type: Array,
      default: () => [],
    },
  },
  components: {
    cardFlip,
  },
  data() {
    return {
      rules: [],
      availWidth: screen.availWidth,
      availHeight: screen.availHeight,
    };
  },
  methods: {
    checkValue(value) {
      this.rules.push(value);
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        this.$refs[`card-${this.rules[0].index}`][0].onEnalBleDisabled();
        this.$refs[`card-${this.rules[1].index}`][0].onEnalBleDisabled();
        this.rules = [];

        // lấy toàn bộ các thẻ đã lật đúng
        const disabledElement = document.querySelectorAll(
          ".screen .card.disabled"
        );
        // so sánh xem đã lật đúng hết chưa
        if (
          disabledElement &&
          disabledElement.length === this.cardContexts.length - 2
        ) {
          // gửi sự kiện cho thằng app
          setTimeout(() => {
            this.$emit("onFinish");
          }, 900);
        }
      } else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        setTimeout(() => {
          this.$refs[`card-${this.rules[0].index}`][0].onFlipCardBack();
          this.$refs[`card-${this.rules[1].index}`][0].onFlipCardBack();
          this.rules = [];
        }, 500);
      } else {
        return false;
      }
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen_inner {
  margin: 2rem auto;
  display: flex;
  flex-wrap: wrap;
}
</style>
