<template>
  <div class="about">
    <div class="left-box">
      <el-table
      :data="resData"
      style="width: 100%">
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
        prop=""
        label="地址">
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
  methods: {
    getData () {
      this.reslist = this.$route.query.data
    }
  },
  mounted () {
    this.getData()
    console.log(this.reslist)
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
