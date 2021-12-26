<template>
  <div id="app">
    <div class="box">
      <div class="middle_box">
        <button class="buttonconvert" @click = "convert" v-show="isshow">切换</button>
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
      value: '',
      isshow: false,
      convertvalue: true,
      data: [],
      sorted_data: []
    }
  },
  mounted () {
    this.isshow = false
  },
  methods: {
    convert () {
      if (this.convertvalue) {
        this.reslist = this.sorted_data
        this.$router.push({ path: '/about', query: { data: this.reslist } })
      } else {
        this.reslist = this.data
        this.$router.push({ path: '/about', query: { data: this.reslist } })
      }
      this.convertvalue = !this.convertvalue
    },
    commit () {
      var obj = {}
      obj.value = this.value
      obj = JSON.stringify(obj)
      this.$axios({
        method: 'post',
        url: 'http://10.108.14.126:8000/search/',
        data: obj,
        headers: {
          'content-type': 'application/json'
        }
      }).then(res => {
        if (res.data.status === 201) {
          alert('未查找到相关的文献内容，请检查后重新输入')
        } else {
          this.data = res.data.data
          this.sorted_data = res.data.sorted_data
          this.reslist = this.data
          console.log(this.reslist)
          this.$router.push({ path: '/about', query: { data: this.reslist } })
          this.isshow = true
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
  justify-content: space-around;
  flex-direction: row;
  align-items:center;
  width:90%;
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
  width:8%;
  height:60%;
  border-radius: 20px;
  font-size:25px;
}
.buttonconvert{
  width:8%;
  height:50%;
  border-radius: 20px;
  font-size:25px;
}
</style>
