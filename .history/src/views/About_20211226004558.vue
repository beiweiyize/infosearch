<template>
  <div class="about">
    <div class="left-box">
      <el-table
      :data="reslist"
      style="width: 100%"
      @row-click='handle'
      highlight-current-row>
      <el-table-column
        prop="title"
        label="论文名称"
        width="180">
      </el-table-column>
      <el-table-column
        prop="year"
        label="年份"
        width="180">
      </el-table-column>
      <el-table-column
        prop="score"
        label="重要性分数">
      </el-table-column>
    </el-table>
    </div>
    <div class="right-box"></div>
  </div>
</template>
<script>
export default {
  data () {
    return {
      reslist: []
    }
  },
  watch: {
    $route: 'getData'
  },
  methods: {
    getData () {
      this.reslist = this.$route.query.data
    },
    handle (row, event, column) {
      var obj = {}
      obj.value = row.Sid
      
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
          this.reslist = res.data.data
          this.$router.push({ path: '/about', query: { data: this.reslist } })
        }
      }).catch(err => {
        console.log(err)
      })
    }
  },
  mounted () {
    this.getData()
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
}
.right-box {
  width: 66%;
  height: 100%;
}
</style>
