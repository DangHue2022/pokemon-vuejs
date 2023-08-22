<template>
  <main-screen
    v-if="statusMatch === '0'"
    @onStart="changeStatusMatch($event)"
  />
  <interact-screen
    v-if="statusMatch === '1'"
    :cardContexts="settings.cardContexts"
    @onFinish="onFinishGame"
  />
  <result-screen
    v-if="statusMatch === '2'"
    :timer="settings.timer"
    @onStartGame="onStartGame"
  />
  <copy-right />
</template>

<script>
import mainScreen from "./components/mainScreen.vue";
import interactScreen from "./components/interactScreen.vue";
import resultScreen from "./components/resultScreen.vue";
import CopyRight from "./components/copyRight.vue";

import { random } from "./ultis/array";
export default {
  name: "App",
  data() {
    return {
      statusMatch: "0",
      settings: {
        totalOfBlocks: 0,
        cardContexts: [],
        startAt: 0,
        timer: 0,
      },
    };
  },
  components: {
    mainScreen,
    interactScreen,
    resultScreen,
    CopyRight,
  },
  methods: {
    changeStatusMatch(event) {
      this.settings.totalOfBlocks = event.totalBlocks;
      const firstArray = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondArray = [...firstArray];
      this.settings.cardContexts = [...firstArray, ...secondArray];
      this.settings.cardContexts = random(
        random(random(random(this.settings.cardContexts)))
      );
      this.settings.startAt = new Date().getTime();
      this.statusMatch = "1";
    },
    onFinishGame() {
      // tính thời gian hoàn thành game
      this.settings.timer = new Date().getTime() - this.settings.startAt;

      // chuyển compoment sang compoment hoàn thành
      this.statusMatch = "2";
    },
    onStartGame() {
      this.statusMatch = "0";
    },
  },
};
</script>
