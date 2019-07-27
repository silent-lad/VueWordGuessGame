<template>
  <div id="app">
    <div class="displayPanel">
      <ImagePanel />
    </div>
    <div :class="state" class="playPanel">
      <div class="words_wrapper">
        <Word
          :key="index"
          v-for="(word, index) in wordArray"
          :alphabet="word"
          :state="state"
        />
      </div>
      <div class="bottom_panel">
        <button class="skip"></button>
        <button class="btn action"></button>
        <button class="backspace"></button>
      </div>
    </div>
  </div>
</template>

<script>
import ImagePanel from "@/components/ImagePanel.vue";
import Slots from "@/components/Slots.vue";
import Word from "@/components/Word.vue";

export default {
  name: "app",
  components: {
    ImagePanel,
    Slots,
    Word
  },
  data() {
    return {
      wordArray: ["a", "b", "c", "d", "e", "f", "i"],
      state: "danger"
    };
  }
};
</script>

<style lang="scss">
$dangerGradient: linear-gradient(to bottom, #ed213a, #93291e);
$warningGradient: linear-gradient(to bottom, #f8de48, #ffbc02);
$successGradient: linear-gradient(to top, #a8e063, #56ab2f);
$primaryGradient: linear-gradient(to top, #0575e6, #021b79);
$blackGradient: linear-gradient(to top, #232526, #414345);

body {
  margin: 0;
}
#app {
  max-width: 450px;
  margin: 0 auto;
  height: 100vh;
  // background: $primaryGradient;
  .displayPanel {
    height: 65%;
  }
  .playPanel {
    height: 35%;
    position: sticky;
    bottom: 0;
    width: 100%;
    margin: 0;
    border-top-left-radius: 15px;
    border-top-right-radius: 15px;

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
  // border-radius: 5%;
}
</style>
