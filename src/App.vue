<template>
  <div id="app"></div>
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
  }
};
</script>

<style lang="scss">
@import url("./variables.scss");
body {
  margin: 0;
}
#app {
  max-width: 500px;
  border: 1px solid red;
  margin: 0 auto;
  height: 100%;
  border-radius: 10%;
}
</style>
