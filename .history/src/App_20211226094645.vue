<template>
  <div id="app">
    <div class="box">
      <div class="middle_box">
        <div class="input_box">
          <input class="input" type="text" placeholder="请输入搜索的文献名称" v-model="value">
          <button class="button" @click = "commit">搜索</button>
        </div>
      </div>
       <router-view />
    </div>
  </div>
</template>

<script>

export default {
  data () {
    return {
      reslist: [],
      value: ''
    }
  },
  methods: {
    commit () {
      var obj = {}
      obj.value = this.value
      obj = JSON.stringify(obj)
      this.$axios({
        method: 'post',
        url: '/api/',
        data: obj,
        headers: {
          'content-type': 'application/json'
        }
      }).then(res => {
        if (res.data.status === 201) {
          alert('未查找到相关的文献内容，请检查后重新输入')
        } else {
          console.log(res.data)
          this.reslist = res.data.data
          this.$router.push({ path: '/about', query: { data: this.reslist } })
        }
      }).catch(err => {
        console.log(err)
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
  flex-direction: column;
  align-items:center;
  height: 950px;
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
 .g6-tooltip {
    border: 1px solid #e2e2e2;
    border-radius: 4px;
    font-size: 12px;
    color: #545454;
    background-color: rgba(255, 255, 255, 0.9);
            padding: 10px 8px;
            box-shadow: rgb(174, 174, 174) 0px 0px 10px
        }
        .minimap{
            position: relative;
            right: 0;
            top: 6px;
            background-color: #fff
        }
</style>
