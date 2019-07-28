<template>
  <div class="img-wrapper">
    <img :src="getImgUrl(imageName)" />
    <p>3/10</p>
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
  top: 7%;
  margin: 0 auto;
  height: 35vh;
  width: 35vh;
  border: 0.5px solid #eaeaea;
  border-radius: 2px;
  padding: 2px;
  background: #eaeaea;
  box-shadow: 3px 3px 4px #333;
  img {
    height: 100%;
    width: 100%;
  }
  p {
    margin: 10px auto;
    color: #767676;
    font-size: 12px;
    text-align: center;
  }
}
</style>
