<template>
  <div :class="states[currentState]" id="app">
    <div class="displayPanel">
      <div class="score">Score: {{ currentScore }}</div>
      <button class="timer">
        {{ time }}
      </button>
      <ImagePanel :imageName="currentName" />
      <WordSlot :state="states[currentState]" :guessArray="guessArray" />
    </div>
    <div :class="states[currentState]" class="playPanel">
      <div class="words_wrapper">
        <Word
          :key="index"
          v-for="(word, index) in wordArray"
          :alphabet="word"
          :state="states[currentState]"
        />
      </div>
      <div class="bottom_panel">
        <button class="btn skip">d</button>
        <button class="btn action">d</button>
        <button class="btn backspace">d</button>
      </div>
    </div>
  </div>
</template>

<script>
import ImagePanel from "@/components/ImagePanel.vue";
import WordSlot from "@/components/WordSlot.vue";
import Word from "@/components/Word.vue";

export default {
  name: "app",
  components: {
    ImagePanel,
    WordSlot,
    Word
  },
  data() {
    return {
      wordArray: ["a", "b", "c", "d", "e", "f", "i"],
      guessArray: ["b", "", "", "", "", "", ""],
      states: ["warning", "danger", "success"],
      currentState: 0,
      currentName: "guru",
      names: ["guru", "football", "silentlad"],
      score: 0,
      time: 0,
      currentScore: 0
    };
  },
  methods: {
    startTimer() {
      this.time = 15;
      var timer = setInterval(() => {
        if (--this.time == 0) {
          clearInterval(timer);
          this.timeOver();
        }
      }, 1000);
    },
    timeOver() {
      console.log("time over");
    }
  },
  created() {
    this.startTimer();
  },
  computed: {}
};
</script>

<style lang="scss">
@import "./variables.scss";

body {
  margin: 0;
}
#app {
  position: relative;
  overflow-y: scroll;
  overflow-x: hidden;
  width: 100vw;
  margin: 0 auto;
  height: 100vh;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  @media screen and(min-width:500px) {
    width: 450px !important;
    box-shadow: 4px 4px 10px #767676;
  }
  .displayPanel {
    overflow-y: scroll;
    overflow-x: hidden;
    .score {
      position: absolute;
      top: 10px;
      right: 10px;
    }
    .timer {
      width: 40px;
      height: 40px;
      margin: 10px 45%;
      border-radius: 40px;
      box-shadow: 1px 1px 40px #eaeaea50;
      background: #eaeaea;
      border: 1px solid #eaeaea;
      text-align: center;
    }
    .slot_wrapper {
      position: relative;
      top: 20%;
      display: flex;
      flex-direction: row;
      justify-content: center;
      flex-wrap: wrap;
      padding: 5% 5%;
      margin: 10px auto;
    }
  }
  .playPanel {
    overflow-y: scroll;
    overflow-x: hidden;
    position: fixed;
    bottom: 0px;
    width: 100%;
    margin: 0 auto;
    background: #eaeaea;
    @media screen and(min-width:500px) {
      width: 450px !important;
    }
    &.success {
      background: $successGradient;
      box-shadow: 0px -20px 50px #56ab2f3a;
    }
    &.danger {
      background: $dangerGradient;
      box-shadow: 0px -20px 50px #b1261745;
    }
    &.warning {
      background: $warningGradient;
      box-shadow: 0px -20px 50px #fdc83034;
    }

    .words_wrapper {
      display: flex;
      flex-direction: row;
      justify-content: center;
      flex-wrap: wrap;
      padding: 6% 6% 0 6%;
    }
  }
  .bottom_panel {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    padding: 5% 7%;
    .btn {
      -webkit-touch-callout: none;
      -webkit-user-select: none;
      -khtml-user-select: none;
      -moz-user-select: none;
      -ms-user-select: none;
      user-select: none;
      outline: none;
    }
    .skip {
      width: 50px;
      height: 50px;
      border: 0.3px solid #ffffff00;
      border-radius: 10px;
      background: $dangerGradient;
      &:active {
        box-shadow: 1px 1px 5px pink;
        transform: translate(2px, 2px);
      }
    }
    .action {
      width: 60%;
      border: 0.3px solid #ffffff00;
      border-radius: 10px;
      background: $primaryGradient;
      &:active {
        box-shadow: 1px 1px 5px $successBorderColor;
        transform: translate(2px, 2px);
      }
    }
    .backspace {
      width: 50px;
      height: 50px;
      border: 0.3px solid #ffffff00;
      border-radius: 10px;
      background: $blackGradient;
      &:active {
        box-shadow: 1px 1px 5px black;
        transform: translate(2px, 2px);
      }
    }
  }
}
</style>
