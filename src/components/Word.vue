<template>
  <div :class="state" class="word success">{{ alphabet }}</div>
</template>

<script>
export default {
  name: "Word",
  props: ["alphabet", "state"],
  data() {
    return {};
  },
  methods: {
    handleClick() {
      this.$emit("WORD_SELCTED", this.alphabet);
    }
  }
};
</script>

<style lang="scss">
@import "../variables.scss";

.word {
  height: 25px;
  width: 25px;
  border-radius: 10px;
  margin: 2% 3%;
  font-size: 23px;
  text-align: center;
  text-transform: uppercase;
  padding: 8px;
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -khtml-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  &.success {
    box-shadow: 3px 3px 10px $successBorderColor;
    background: $successBackgroundColor;
    color: $successTextColor;
    &:active {
      box-shadow: 1px 1px 5px $successBorderColor;
      transform: translate(2px, 2px);
    }
  }
  &.danger {
    box-shadow: 3px 3px 10px $dangerBorderColor;
    background: $dangerBackgroundColor;
    color: $dangerTextColor;
    &:active {
      box-shadow: 1px 1px 5px $dangerBorderColor;
    }
  }
  &.warning {
    box-shadow: 3px 3px 10px $warningBorderColor;
    background: $warningBackgroundColor;
    color: $warningTextColor;
    &:active {
      box-shadow: 1px 1px 5px $warningBorderColor;
    }
  }
}
</style>
