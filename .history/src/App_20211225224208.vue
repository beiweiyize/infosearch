<template>
  <div id="app">
    <div class="box">
      <div class="middle_box">
        <div class="input_box">
          <input class="input" type="text" placeholder="请输入搜索的文献名称">
          <button class="button" @click = "commit">搜索</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      reslist: []
    }
  },
  methods: {
    commit () {
      this.$axios({
        method: 'post',
        url:
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* background-image: url("../assets/background.png"); */
}

/* #nav {
  padding: 30px;
}
.middle_box{

} */
.box {
  display: flex;
  justify-content: center;
  flex-direction: row;
  align-items:center;
  height: 1000px;
  width: 100%;
  background-size: cover;
  background-repeat: no-repeat;
}

.middle_box{
  display: flex;
  justify-content: center;
  flex-direction: row;
  align-items:center;
  width:60%;
  height:10%;
}

.input_box{
  display: flex;
  justify-content: space-between;
  flex-direction: row;
  align-items:center;
  width:80%;
  height:80%;
}
.input{
  width:70%;
  height:60%;
  border-radius: 20px;
  font-size:25px;
  text-align: center;
}
.button{
  width:18%;
  height:60%;
  border-radius: 20px;
  font-size:25px;
}
</style>
