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
.word {
  height: 7vw;
  width: 7vw;
  border-radius: 10px;
  margin: 1% 3%;
  font-size: 25px;
  text-align: center;
  text-transform: uppercase;
  font-family: "Gill Sans", "Gill Sans MT", Calibri, "Trebuchet MS", sans-serif;
  padding: 1.5vw;
  &.success {
    border: 5px solid #366d1d;
    background: #bcff9e;
    color: #264218;
  }
  &.danger {
    border: 1px solid;
  }
  &.warning {
    border: 1px solid;
  }
}
</style>
