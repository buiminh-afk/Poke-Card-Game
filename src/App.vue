<template>
  <div>
    <MainScreen 
      v-if="gameStatus === 'default'" 
      @onStart="beforeStart($event)"
    />
    <InteractScreen 
      v-if="gameStatus === 'playing'" 
      :cardsContext="settings.cardsContext"
      @onFinish="onGetResult"
    />
    <ResultScreen
      v-if="gameStatus === 'result'"
      :timer="timer"
      @onStartAgain = "startAgain"
    />
    <CopyRight/>
  </div>
</template>

<script>
import MainScreen from './components/MainScreen.vue'
import InteractScreen from './components/InteractScreen.vue'
import ResultScreen from './components/ResultScreen.vue'
import CopyRight from './components/CopyRight.vue'
import { shuffle } from './utils/array.js'

export default {
  name: "App",
  data() {
    return {
      settings : {
        totalCards : 0,
        cardsContext : [],
        startTime : null,
      },
      gameStatus : "default", 
      timer : 0,
    }
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
    CopyRight
  },
  methods : {
    beforeStart(config) {
      this.settings.totalCards = config.totalCards;

      const firstCard = Array.from({ length : this.settings.totalCards / 2 }, (_ , i) => i+1);
      const secondCard = [...firstCard];
      const cards = [...firstCard,...secondCard];
      this.settings.cardsContext = shuffle(shuffle(cards));
      this.settings.startTime = new Date().getTime();
      this.gameStatus = "playing";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startTime;
      this.gameStatus = "result";
    },
    startAgain() {
      this.gameStatus = "default";
    }
  },
};
</script>

<style scoped>

</style>
