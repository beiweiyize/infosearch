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
    <div class="right-box">
      <div id="mountNode"></div>
    </div>
  </div>
</template>
<script>
import G6 from '@antv/g6'
export default {
  data () {
    return {
      reslist: [],
      Nodelist: [],
      graph: null
    }
  },
  watch: {
    $route: 'getData'
  },
  methods: {
    Graph () {
      G6.registerNode(
        'background-animate',
        {
          afterDraw (cfg, group) {
            let r = cfg.size / 2
            if (isNaN(r)) {
              r = cfg.size[0] / 2
            }
            // 第一个背景圆
            const back1 = group.addShape('circle', {
              zIndex: -3,
              attrs: {
                x: 0,
                y: 0,
                r,
                fill: cfg.color,
                opacity: 0.6
              },
              // must be assigned in G6 3.3 and later versions. it can be any value you want
              name: 'circle-shape1'
            })
            // 第二个背景圆
            const back2 = group.addShape('circle', {
              zIndex: -2,
              attrs: {
                x: 0,
                y: 0,
                r,
                fill: 'blue', // 为了显示清晰，随意设置了颜色
                opacity: 0.6
              },
              // must be assigned in G6 3.3 and later versions. it can be any value you want
              name: 'circle-shape2'
            })
            // 第三个背景圆
            const back3 = group.addShape('circle', {
              zIndex: -1,
              attrs: {
                x: 0,
                y: 0,
                r,
                fill: 'green',
                opacity: 0.6
              },
              // must be assigned in G6 3.3 and later versions. it can be any value you want
              name: 'circle-shape3'
            })
            group.sort() // 排序，根据 zIndex 排序
            // 第一个背景圆逐渐放大，并消失
            back1.animate(
              {
                r: r + 20,
                opacity: 0.1
              },
              {
                repeat: true, // 循环
                duration: 3000,
                easing: 'easeCubic',
                delay: 0// 无延迟
              }
            )
            // 第二个背景圆逐渐放大，并消失
            back2.animate(
              {
                r: r + 20,
                opacity: 0.1
              },
              {
                repeat: true, // 循环
                duration: 3000,
                easing: 'easeCubic',
                delay: 1000 // 1 秒延迟
              }
            ) // 1 秒延迟

            // 第三个背景圆逐渐放大，并消失
            back3.animate(
              {
                r: r + 20,
                opacity: 0.1
              },
              {
                repeat: true, // 循环
                duration: 3000,
                easing: 'easeCubic',
                delay: 2000 // 2 秒延迟
              }
            )
          }
        },
        'circle'
      )
      G6.registerEdge(
        'line-growth',
        {
          afterDraw (cfg, group) {
            const shape = group.get('children')[0]
            const length = shape.getTotalLength()
            shape.animate(
              (ratio) => {
                // the operations in each frame. Ratio ranges from 0 to 1 indicating the prograss of the animation. Returns the modified configurations
                const startLen = ratio * length
                // Calculate the lineDash
                const cfg = {
                  lineDash: [startLen, length - startLen]
                }
                return cfg
              },
              {
                repeat: false, // Whether executes the animation repeatly
                duration: 3000 // the duration for executing once
              }
            )
          }
        },
        'cubic' // extend the built-in edge 'cubic'
      )
      const minimap = new G6.Minimap({
        size: [150, 150],
        className: 'minimap',
        type: 'delegate'
      })
      const grid = new G6.Grid()
      this.graph = new G6.Graph({
        container: 'mountNode',
        animate: true,
        plugins: [minimap, grid],
        width: 1267.19,
        height: 1300,
        fitView: true,
        fitViewPadding: [0, 0, 0, 0],
        layout: {
          type: 'force', // 指定为力导向布局
          preventOverlap: true, // 防止节点重叠
          linkDistance: 500, // 指定边距离为100
          center: [400, 400]
        },
        modes: {
          default: ['drag-canvas', 'zoom-canvas', 'drag-node',
            {
              type: 'tooltip',
              formatText (model) {
              // 提示框文本内容
                const text = 'title: ' + model.title + '<br/> year: ' + model.year + '<br/> score: ' + model.score
                // const text = 'label: ' + model.label + '<br/> class: ' + model.class
                return text
              }
            }] // 允许拖拽画布、放缩画布、拖拽节点
        },
        nodeStateStyles: {
          // 鼠标 hover 上节点，即 hover 状态为 true 时的样式
          hover: {
            fill: 'lightsteelblue'
          },
          // 鼠标点击节点，即 click 状态为 true 时的样式
          click: {
            stroke: '#000',
            lineWidth: 3
          }
        },
        // 边不同状态下的样式集合
        edgeStateStyles: {
          // 鼠标点击边，即 click 状态为 true 时的样式
          click: {
            stroke: 'steelblue'
          }
        }
      })
      const main = async () => {
        const data = this.Nodelist
        const nodes = data.nodes
        const edges = data.edges
        nodes.forEach((node) => {
          if (!node.style) {
            node.style = {}
          }
          node.style.lineWidth = 1
          node.style.stroke = '#66000f'
          node.style.fill = 'steelblue'
          node.labelCfg = {
            style: {
              fontSize: 30,
              fill: '#fff' // 节点标签文字颜色
            },
            position: 'center'
          }
          switch (node.class) {
            case 'c0': {
              node.type = 'background-animate'
              node.size = 200
              break
            }
            case 'c1': {
              node.type = 'circle'
              node.size = 150
              break
            }
            case 'c2': {
              node.type = 'circle'
              node.size = 100
              break
            }
            case 'c3': {
              node.type = 'circle'
              node.size = 50
              break
            }
          }
        })
        edges.forEach((edge) => {
          if (!edge.style) {
            edge.style = {}
          }
          edge.style.lineWidth = 2
          edge.style.opacity = 1
          edge.style.stroke = 'grey'
          edge.type = 'line-growth'
          edge.labelCfg = {
            autoRotate: true
          }
        })
        this.graph.data(data)
        this.graph.render()
        // 监听鼠标进入节点
        this.graph.on('node:mouseenter', (e) => {
          const nodeItem = e.item
          // 设置目标节点的 hover 状态 为 true
          this.graph.setItemState(nodeItem, 'hover', true)
        })
        // 监听鼠标离开节点
        this.graph.on('node:mouseleave', (e) => {
          const nodeItem = e.item
          // 设置目标节点的 hover 状态 false
          this.graph.setItemState(nodeItem, 'hover', false)
        })
        // 监听鼠标点击节点
        this.graph.on('node:click', (e) => {
          // 先将所有当前有 click 状态的节点的 click 状态置为 false
          const clickNodes = this.graph.findAllByState('node', 'click')
          clickNodes.forEach((cn) => {
            this.graph.setItemState(cn, 'click', false)
          })
          const nodeItem = e.item
          // 设置目标节点的 click 状态 为 true
          this.graph.setItemState(nodeItem, 'click', true)
        })
        // 监听鼠标点击节点
        this.graph.on('edge:click', (e) => {
          // 先将所有当前有 click 状态的边的 click 状态置为 false
          const clickEdges = this.graph.findAllByState('edge', 'click')
          clickEdges.forEach((ce) => {
            this.graph.setItemState(ce, 'click', false)
          })
          const edgeItem = e.item
          // 设置目标边的 click 状态 为 true
          this.graph.setItemState(edgeItem, 'click', true)
        })
      }
      main()
    },
    getData () {
      this.reslist = this.$route.query.data
    },
    handle (row, event, column) {
      if (this.graph != null) {
        this.graph.destroy()
      }
      console.log(row.Sid)
      var obj = {}
      obj.value = row.Sid
      obj = JSON.stringify(obj)
      this.$axios({
        method: 'post',
        url: '/draw/',
        data: obj,
        headers: {
          'content-type': 'application/json'
        }
      }).then(res => {
        this.Nodelist = res.data
        this.Graph()
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
.g6-tooltip {
    border: 1px solid #e2e2e2;
    border-radius: 4px;
    font-size: 18px;
    color: #545454;
    background-color: rgba(255, 255, 255, 0.9);
    padding: 10px 8px;
    box-shadow: rgb(174, 174, 174) 0px 0px 10px
}
.minimap{
    position: absolute;
    right: 0;
    top: 80px;
    background-color: #fff
}
</style>
