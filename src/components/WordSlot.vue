<template>
  <div class="slot_wrapper">
    <div
      :key="index"
      v-for="index in guessLength"
      :class="[
        state,
        index < activeIndex != '' ? 'typed' : '',
        activeIndex == index ? 'active' : ''
      ]"
      class="slots"
    >
      {{ guessArray[index - 1] }}
    </div>
  </div>
</template>

<script>
export default {
  name: "WordSlot",
  props: ["guessLength", "state", "guessArray"],
  computed: {
    activeIndex() {
      var activeIndex = 0;
      var selected = 0;
      return this.guessArray.length + 1;
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
    box-shadow: 3px 3px 1px #d4d3d3;
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
