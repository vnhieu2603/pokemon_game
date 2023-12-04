<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"></main-screen>
  <interact-screen v-if="statusMatch === 'match'"></interact-screen>
  <copy-right-screen/>
</template>

<script>
import MainScreen from "./components/MainScreen.vue"
import InteractScreen from "./components/InteractScreen.vue"
import CopyRightScreen from "./components/CopyRightScreen.vue";

export default {
  name: "App",
  data() {
    return {

      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
      },
      statusMatch: "default",
    }
  },
  components: {
    MainScreen,
    InteractScreen,
    CopyRightScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("running handle before start, ", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;
      //data ready
      this.statusMatch = "match";
    }
  }
};
</script>

<style></style>
