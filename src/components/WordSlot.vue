<template>
  <div class="slot_wrapper">
    <div
      :key="index"
      v-for="(alphabet, index) in guessArray"
      :class="[
        state,
        alphabet != '' ? 'typed' : '',
        activeIndex == index ? 'active' : ''
      ]"
      class="slots"
    >
      {{ alphabet }}
    </div>
  </div>
</template>

<script>
export default {
  name: "WordSlot",
  props: ["guessArray", "state"],
  computed: {
    activeIndex() {
      var activeIndex = 0;
      var selected = 0;
      this.guessArray.forEach((el, index) => {
        if (el == "" && !selected) {
          activeIndex = index;
          selected++;
        }
      });
      return activeIndex;
    }
  }
};
</script>

<style lang="scss">
@import "../variables.scss";

.slots {
  height: 20px;
  width: 20px;
  border-radius: 7px;
  margin: 1% 1%;
  font-size: 18px;
  text-align: center;
  text-transform: uppercase;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  background: #eaeaea;
  border: 2px solid #ffffff00;
  padding: 7px;
  &.typed {
    &.success {
      border: 2px solid $successBorderColor;
      color: $successTextColor;
      background: $successBackgroundColor;
    }
    &.danger {
      border: 2px solid $dangerBorderColor;
      color: $dangerTextColor;
      background: $dangerBackgroundColor;
    }
    &.warning {
      border: 2px solid $warningBorderColor;
      color: $warningTextColor;
      background: $warningBackgroundColor;
    }
  }
  &.active {
    &.success {
      border: 2px solid $successBorderColor;
    }
    &.danger {
      border: 2px solid $dangerBorderColor;
    }
    &.warning {
      border: 2px solid $warningBorderColor;
    }
  }
}
</style>
