<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="onHandleBeforeStart($event)"
  />
  <interact-screen
    v-if="statusMatch === 'match'"
    :cardContext="settings.cardContext"
    @onFinished="onGetResult"
  />
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  />
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffled } from "./utils/array";

export default {
  name: "App",
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("run", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCard = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      );

      const secondCard = [...firstCard];

      const cards = [...firstCard, ...secondCard];

      this.settings.cardContext = shuffled(shuffled(shuffled(cards)));
      this.settings.startedAt = new Date().getTime();

      this.statusMatch = "match";
    },
    onGetResult() {
      // get timer
      this.timer = new Date().getTime() - this.settings.startedAt;
      // result
      this.statusMatch = "result";
    },
  },
};
</script>