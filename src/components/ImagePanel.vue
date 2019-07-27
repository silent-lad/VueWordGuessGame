<template>
  <div class="img-wrapper">
    <img :src="getImgUrl('guru')" />
  </div>
</template>

<script>
export default {
  name: "ImagePanel",
  props: ["imageName"],
  methods: {
    getImgUrl(pic) {
      return require("../assets/" + pic + ".jpg");
    }
  }
};
</script>

<style lang="scss">
.img-wrapper {
  position: relative;
  top: 10%;
  margin: 0 auto;
  height: 70vw;
  width: 70vw;
  border: 0.5px solid #eaeaea;
  border-radius: 2px;
  padding: 2px;
  box-shadow: 2px 2px 10px #eaeaea;
  img {
    height: 100%;
    width: 100%;
  }
}
</style>
