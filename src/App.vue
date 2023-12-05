<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"></main-screen>
  <interact-screen 
      v-if="statusMatch === 'match'" 
      :cardsContext="settings.cardsContext"
      @onFinish="onGetResult">
  </interact-screen>
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="statusMatch = 'default'"></result-screen>
  <!-- <copy-right-screen/> -->
</template>

<script>
import MainScreen from "./components/MainScreen.vue"
import InteractScreen from "./components/InteractScreen.vue"
import CopyRightScreen from "./components/CopyRightScreen.vue"
import ResultScreen from "./components/ResultScreen.vue"

import {shuffled} from './utils/array'
export default {
  name: "App",
  data() {
    return {

      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    }
  },
  components: {
    MainScreen,
    InteractScreen,
    // CopyRightScreen,
    ResultScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("running handle before start, ", config);
      console.log("config: " + config);
      this.settings.totalOfBlocks = config.totalOfBlocks;

      const firstCards = Array.from(
        { length: this.settings.totalOfBlocks / 2 },
        (_, i) => i + 1
      )
      const secondCards = [...firstCards];
      const mix = [...firstCards, ...secondCards]

      this.settings.cardsContext = shuffled(shuffled(shuffled(mix)));
      this.settings.startedAt = new Date().getTime();
      //data ready
      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt;

      this.statusMatch = "result";
    }
  }
};
</script>

<style></style>
