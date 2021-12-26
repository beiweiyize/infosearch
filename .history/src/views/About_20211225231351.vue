<template>
  <div class="about">
    <div class="left-box"></div>
    <div class="right-box"></div>
  </div>
</template>
<script>
export default {
  data () {
    return {
    }
  },
  methods: {
    getData(){

    }
  },
  mounted() {

  }
}
</script>
<style>
.about {
  width: 1920px;
  height: 860px;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
}
.left-box {
  width: 33%;
  height: 100%;
  background-color: blue;
}
.right-box {
  width: 66%;
  height: 100%;
  background-color: red;
}
</style>
