<template>
  <div :class="states[currentState]" id="app">
    <div class="displayPanel">
      <div class="score">Score: {{ score }}</div>
      <button class="timer" :class="{ pulse: playing }">
        {{ time }}
      </button>
      <ImagePanel
        :imageName="names[currentNameIndex]"
        :total="names.length"
        :current="currentNameIndex + 1"
      />
      <WordSlot
        :state="states[currentState]"
        :guessLength="guessLength"
        :guessArray="guessArray"
      />
    </div>
    <div :class="states[currentState]" class="playPanel">
      <div v-if="playing" class="words_wrapper">
        <Word
          :key="index"
          v-for="(word, index) in wordArray"
          :alphabet="word"
          :state="states[currentState]"
          @click.native="selectWord(word, index)"
        />
      </div>
      <div v-else class="info_box" :class="isCorrect ? 'isCorrect' : 'isWrong'">
        <i v-if="isCorrect" class="far fa-smile"></i>
        <i v-else class="far fa-frown"></i>
        {{ infoMessage }}
      </div>
      <div v-if="playing" class="bottom_panel">
        <span>
          <button :disabled="skipCount == 0" @click="skip()" class="btn skip">
            <i class="fas fa-forward"></i>
          </button>
          <br />
          <div class="label">Skip: {{ skipCount }}</div>
        </span>

        <button
          :disabled="guessLength != guessArray.length"
          @click="submit()"
          class="btn action"
        >
          Submit
        </button>
        <span>
          <button
            :disabled="guessArray.length == 0"
            @click="deleteWord()"
            class="btn backspace"
          >
            <i class="fas fa-backspace"></i>
          </button>
          <br />
          <div class="label">Clear</div>
        </span>
      </div>
      <div v-else class="bottom_panel">
        <button
          v-if="currentNameIndex == 2"
          @click="endGame()"
          class="btn action"
        >
          New Game
        </button>
        <button v-else @click="nextRound()" class="btn action">
          Next Question
        </button>
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
      gameStage: 0,
      wordArray: [],
      guessArray: [],
      states: ["warning", "danger", "success"],
      currentState: 0,
      currentNameIndex: 0,
      names: ["guru", "football", "silentlad"],
      time: 0,
      timer: null,
      score: 0,
      playing: true,
      infoMessage: "That's Correct!",
      isCorrect: false,
      responses: [
        "That's Correct!",
        "Oops Wrong Answer!",
        "Oops, Time Ran out!",
        "Question Skipped"
      ],
      skipCount: 1
    };
  },

  methods: {
    selectWord(word, index) {
      this.guessArray.push(word);
      this.wordArray.splice(index, 1);
    },
    deleteWord() {
      if (this.guessArray) {
        this.wordArray.push(this.guessArray.pop());
      }
    },
    startGame() {
      this.guessArray = [];
      this.currentNameIndex = 0;
      this.playing = true;
      this.skipCount = 1;
      this.wordArray = this.shuffle(
        this.names[this.currentNameIndex].split("")
      );
      this.startTimer();
    },
    checkAnswer() {
      if (this.guessArray.length != this.guessLength) return false;
      if (this.guessArray.join("") == this.names[this.currentNameIndex]) {
        return true;
      } else {
        return false;
      }
    },
    submit() {
      if (this.checkAnswer()) {
        this.score += this.time;
        this.score += 200;
        this.isCorrect = true;
        this.currentState = 2;
        this.infoMessage = this.responses[0];
      } else if (this.time == 0) {
        this.isCorrect = false;
        this.currentState = 1;
        this.infoMessage = this.responses[2];
      } else {
        this.isCorrect = false;
        this.currentState = 1;
        this.infoMessage = this.responses[1];
      }
      this.endTimer();
      this.playing = false;
    },
    nextRound() {
      if (this.currentNameIndex == this.names.length - 1) {
        this.endGame();
        return;
      }
      this.currentNameIndex++;
      this.playing = true;
      this.guessArray = [];
      this.startTimer();
    },
    endGame() {
      this.startGame();
    },
    skip() {
      this.isCorrect = true;
      this.currentState = 2;
      this.infoMessage = this.responses[3];
      this.playing = false;
      this.skipCount--;
      this.endTimer();
    },
    startTimer() {
      this.currentState = 0;
      this.time = 30;
      this.timer = null;
      this.timer = setInterval(() => {
        if (this.time <= 0) {
          this.submit();
        } else {
          this.time -= 1;
        }
      }, 1000);
    },
    endTimer() {
      clearInterval(this.timer);
      this.time = 0;
      this.timer = null;
    },
    shuffle(arra1) {
      var ctr = arra1.length,
        temp,
        index;
      while (ctr > 0) {
        index = Math.floor(Math.random() * ctr);
        ctr--;
        temp = arra1[ctr];
        arra1[ctr] = arra1[index];
        arra1[index] = temp;
      }
      return arra1;
    }
  },

  watch: {
    time(val) {
      if (val == 0) {
        this.playing = false;
      }
    },
    currentNameIndex(val) {
      this.wordArray = this.shuffle(this.names[val].split(""));
    }
  },

  created() {
    this.wordArray = this.shuffle(this.names[this.currentNameIndex].split(""));
    this.startTimer();
  },

  computed: {
    guessLength() {
      return this.names[this.currentNameIndex].split("").length;
    }
  }
};
</script>

<style lang="scss">
@import "./variables.scss";

body {
  margin: 0;
}
.label {
  font-size: 12px;
  color: #695a17ee;
  width: 100%;
  font-weight: 600;
  padding-top: 5px;
  text-align: center;
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
      border: 1px solid #989898;
      border-radius: 40px;
      box-shadow: 1px 1px 40px #eaeaea50;
      background: #eaeaea;
      border: 1px solid #eaeaea;
      text-align: center;
      font-weight: 600;
      font-size: 18px;
      &.pulse {
        animation: pulse 1s infinite;
      }
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
    min-height: 150px;
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
      min-height: 100px;
    }
    .info_box {
      display: flex;
      flex-direction: column;
      justify-content: space-around;
      align-items: center;
      text-align: center;
      flex-wrap: wrap;
      padding: 6% 6% 0 6%;
      min-height: 100px;
      font-size: 35px;
      color: white;
      &.isCorrect {
        text-shadow: 2px 2px green;
      }
      &.isWrong {
        text-shadow: 2px 2px darkred;
      }
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
      &:disabled {
        opacity: 0.4;
      }
    }
    .skip {
      width: 50px;
      height: 50px;
      border: 0.3px solid #ffffff00;
      border-radius: 10px;
      background: $dangerGradient;
      box-shadow: 3px 3px 1px #741707;
      color: white;
      font-size: 20px;
      font-weight: 550;
      &:active {
        box-shadow: 1px 1px 5px #741707;
        transform: translate(2px, 2px);
      }
    }
    .action {
      width: 60%;
      border: 0.3px solid #ffffff00;
      border-radius: 10px;
      height: 50px;
      margin: 0 auto;
      background: $primaryGradient;
      color: white;
      font-size: 20px;
      font-weight: 550;
      box-shadow: 3px 3px 1px #0b3b6b;
      &:active {
        box-shadow: 1px 1px 5px $successBorderColor;
        transform: translate(2px, 2px);
      }
      &.end {
      }
    }
    .backspace {
      width: 50px;
      height: 50px;
      border: 1px solid #323435;
      border-radius: 10px;
      background: $blackGradient;
      box-shadow: 3px 3px 1px #323435;
      color: white;
      font-size: 20px;
      font-weight: 550;
      &:active {
        box-shadow: 1px 1px 5px black;
        transform: translate(2px, 2px);
      }
    }
  }
  @keyframes pulse {
    0% {
      transform: scale(1.1, 1.1);
    }
    100% {
      transform: scale(1, 1);
    }
  }
}
</style>
