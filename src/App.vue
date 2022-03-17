<template>
  <div id="app">
      <div style="display: flex">
        <ul>
          <li draggable="true" v-for="node in nodeSource" :key="node.id" @dragstart="onDrag($event, node.pen)">
            <img src="./assets/logo.png"  height="15" width="20" style="inline-block"/>
            {{ node.name }}
          </li>
        </ul>
        <div id="topology" style="height: 750px; margin: 0 20px;flex: 1"></div>
      </div>
    <!-- <Le5le></Le5le> -->
  </div>
</template>

<script>
import { Topology, Node } from '@topology/core'
// import Le5le from './components/le5le.vue'

export default {
  name: 'App',
  components: {
    // Le5le
  },
  computed: {
    curNodesList() {
      return this.topology.data.pens.filter(pen => pen.type === 0) || []
    },
    curLineList() {
      return this.topology.data.pens.filter(pen => pen.type) || []
    },
    scale() {
      return this.topology.data.scale
    }
  },
  data() {
    return {
      // curNodesList: [],
      topology: null,
      scaleX: 1,
      scaleY: 1,
      nodeSource: [
        {
          id: 1,
          name: 'level1',
          pen: {
              name: 'rectangle',
              // id: '202111111404',
              rect: {
                  // 'x': 25,
                  // 'y': 25,
                  'width': 250,
                  'height': 140
              },

              // 文字
              text: '虚拟机1',
              textMaxLine: 1,
              font: {
                  color: 'black',
                  textAlign: 'left',
                  textBaseline: 'top'
              },
              textOffsetX: 10,
              textOffsetY: 10,

              // 边框和填充
              lineWidth: 3,
              strokeStyle: '#777',
              fillStyle: '#fff',
              borderRadius: 0.01,
              level: 1,
              unique_id: '',
              config: {
                  front_form_version: ''
              },
              // image: require('@/assets/logo.png'),
              // imageWidth: 50,
              // imageHeight:50,
              // imageAlign: 'left-top',
              // iconRect: {
              //   x: 10,
              //   y: 10
              // },
              // 是否锁定
              locked: 0,
              // 自定义数据
              data: null
        }
        },
        {
          id: 2,
          name: 'level2',
          pen: {
              // id: '202111111404',
              rect: {
                  // 'x': 25,
                  // 'y': 25,
                  'width': 200,
                  'height': 100
              },
              font: {
                  color: 'black',
                  textAlign: 'left',
                  textBaseline: 'top'
              },
              textOffsetX: 10,
              textOffsetY: 10,
              strokeStyle: '#777',
              fillStyle: '#fff',
              name: 'rectangle',
              borderRadius: 0.01,
              text: '虚拟机2',
              textMaxLine: 1,
              level: 2,
              unique_id: '',
              config: {
                  front_form_version: ''
              }
        }
        },
        {
          id: 3,
          name: 'level3',
          pen: {
              // id: '202111111404',
              rect: {
                  // 'x': 25,
                  // 'y': 25,
                  'width': 150,
                  'height': 50
              },
              font: {
                  color: 'black',
                  textAlign: 'left',
                  textBaseline: 'top'
              },
              textOffsetX: 10,
              textOffsetY: 10,
              strokeStyle: '#777',
              fillStyle: '#fff',
              name: 'rectangle',
              borderRadius: 0.01,
              text: '虚拟机3',
              textMaxLine: 1,
              level: 3,
              config: {
                  front_form_version: ''
              }
        }
        },
        {
          id: 4,
          name: 'level4',
          pen: {
              rect: {
                  'width': 100,
                  'height': 30
              },
              font: {
                  color: 'black',
                  textAlign: 'left',
                  textBaseline: 'top'
              },
              textOffsetX: 10,
              textOffsetY: 10,
              strokeStyle: '#777',
              fillStyle: '#fff',
              name: 'rectangle',
              borderRadius: 0.01,
              text: '虚拟机4',
              textMaxLine: 1,
              level: 4,
              config: {
                  front_form_version: ''
              }
        }
        }
        ]
    }
  },
  mounted() {
    this.init()
    console.log(this.topology)
    this.topology.data.lineName = 'polyline'
    // sessionStorage.setItem('pure_data', '1111')
  },
  beforeDestroy() {
    // const pure_data = this.topology.data()
    // sessionStorage.setItem('pure_data', pure_data)
  },
  methods: {
    init() {
        // const topologyOptions = {
        //     // 节点、连线数据集合
        //     // pens: [],
        //     // 默认连线类型
        //     lineName: 'line',
        //     // 宽、高  ??感觉没有用
        //     // width: 900,
        //     // height: 900,
        //     // viewPadding: 20,  // 画布可视区域padding，可以是number,string, number_array类型
        //     // 画布偏移量
        //     x: 0,
        //     y: 0,
        //     // 缩放比例、缩放最大值、最小值
        //     scale: 1.0,
        //     minScale: 0.3,
        //     maxScale: 5,
        //     // 图形颜色
        //     // color: '#000000',
        //     // activeColor: '',
        //     // hoverColor: '',
        //     // dragColor: '',
        //     // 画布背景色
        //     // bkColor: '#E02727', //不生效？为什么？,s
        //     // bkImage: '',
        //     // bkImageStatic: false, // 画布背景是否跟随一起移动
        //     // 锚点半径大小|、填充色
        //     anchorRadius: 1,
        //     anchorFillStyle: '#ffffff',
        //     autoAnchor: true, // 连线自动选择最近锚点
        //     // 连接线
        //     // dockStrokeStyle: '', // 连接线到达终点锚点的边框颜色
        //     // dockFillStyle: '',  // 连接线到达终点锚点的填充颜色
        //     // animateColor: '#000000', // 连线动画默认颜色
        //     // 文字 - 两种方式
        //     // font: {
        //     //     color: '',
        //     //     fontFamily: '',
        //     //     fontSize: '',
        //     //     lineHeight: '',
        //     //     textAlign: '',
        //     //     textBaseline: '',
        //     // },
        //     fontColor: '#ff00ff',
        //     // fontFamily: '',
        //     // fontSize: '',
        //     // lineHeight: '',
        //     // textAlign: '',
        //     // textBaseline: '',
        //     // 网格
        //     grid: true,
        //     gridColor: '#FD6585',
        //     gridSize: 30,
        //     // 标尺
        //     rule: true,
        //     ruleColor: '#000000',
        //     // 滚动展示, 为 true 时，滚轮滚动为上下移动画布；为 false 按下 ctrl 键滚轮滚动为缩放画布。
        //     scroll: false,
        //     // 锁定
        //     /**
        //      * None      (0)  - 未锁定，可任意编辑。
        //      * Readonly  (1)  - 只读模式，允许选中
        //      * NoMove    (2)  - 不能移动
        //      * NoEvent   (10) - 禁止鼠标交互，无法做任何操作。纯静态画面模式。
        //      */
        //     // locked: 0,
        //     // 是否显示提示, 默认显示提示(true)
        //     // tooltip: true,
        //     // 可自定义数据
        //     // data: null,
        //     // 一些禁止操作
        //     // hideInput: false,  // 是否禁止双击节点文字，出现文字输入框
        //     // hideRotateCP: false,  // 是否隐藏节点旋转控制点
        //     // hideSizeCP: false,  // 是否隐藏节点大小控制点
        //     // disableSizeX: false,  // 控制点只能改变节点width
        //     // disableSizeY: false,  // 控制点只能改变节点height
        //     // hideAnchor: false,  // 是否隐藏节点锚点
        //     // alwaysAnchor: false,   // 总是显示所有节点的锚点,单独节点不显示锚点时，设置节点hideAnchor
        //     // disableScale: false, // 是否禁止鼠标滚轮缩放，用户仍然可以使用scale函数，以定制缩放交互
        //     disableTranslate: true,  // 是否禁止默认鼠标移动画布
        //     // disableDockLine: false,  // 是否禁用移动节点时，自动停靠
        //     disableEmptyLine: true,  // 是否禁止连线终点为空（未连接到节点）
        //     // 平移画布快捷键类型，默认空格或Ctrl
        //     /** 平移画布时，是否同时需要Ctrl/Shift/Alt等功能键
        //       * Any = -1,  不需要功能键就生效
        //       *  CtrlOrAlt,
        //       *  Ctrl,
        //       *  Shift,
        //       *  Alt,
        //     */
        //     // translateKey: -1,
        //     // 缩放画布快捷键类型
        //     // scaleKey: -1,
        //     // 接收画布消息的回调函数, event - 消息名, data - 消息内容
        //     /**
        //      * this.canvasOptions.on = this.onMessage ;
        //      * onMessage = (event, data) => {};
        //      */
        //     // on: (event, data) => {
        //     //   console.log(event, data)
        //     // },
        //     // 画布监控监听对象。默认监听Document文档
        //     /**
        //      * None = -1,
        //      * Document,
        //      * Canvas,
        //      */
        //     // keydown: 'Document'

        // }
        // options的参数
        const topologyOptions= {
            // 活动层颜色
            hoverColor: 'green',
            // // 是否禁止双击节点文字，出现文字输入框
            hideInput: true,
            // 是否隐藏节点旋转控制点
            hideRotateCP: true,
            // // 是否隐藏节点大小控制点
            hideSizeCP: true,
            // 是否禁止连线终点为空（未连接到节点）
            disableEmptyLine: true,
            // 是否开启：连线自动选择最近锚点
            autoAnchor: true,
            // // 是否显示网格
            grid: true,
            // // 背景网格颜色
            gridColor: '#ccc',
            // // 背景网格格子大小
            gridSize: 25,
            // // 是否显示背景标尺
            rule: true,
            disableTranslate: false,
            on: this.onMessage
        }
        this.topology = new Topology('topology', topologyOptions)
        this.topology.render()
    },
    onDrag(event, pen) {
      event.dataTransfer.setData('Text', JSON.stringify(pen))
    },
    onMessage(event, node) {
      switch (event) {
        case 'addNode': 
          this.addNode(node)
          break
        case 'addLine': 
          this.addLine(node)
          break
        default:
          break
      }
    },
    addNode(node) {
      const { x, y, width, height } = node.rect
      // 鼠标点位置
      const point = {x: x + 1/2 * width, y: y + 1/2 * height}
      let hitNode = null
      // 思路: hitNode 为 null 时， 判断当前层级，如果是 level1 则可以放置
      // 如果 hitNode 不为 null， 判断当前层级大于 hitNode 层级，则可放置，否则不可放置
     this.curNodesList.slice(0, -1).some(pen => {
       hitNode = pen.hit(point)
       return hitNode
       })
      if (!hitNode) {
        if (node.level === 1) {
          console.log('首层放置')
        } else {
          alert('首层非法放置')
          this.topology.delete()
        }
      } else {
        if (node.level > hitNode.level) {
          console.log('成功放置')
          this.pushNode(node, hitNode)
        } else {
          alert('层级关系非法放置')
          this.topology.delete()
        }
      }
    },
    pushNode(node, hitNode) {
      this.topology.delete()
      const newNode = new Node(node)
      const hitNodeLen = hitNode.children.length
      newNode.rectInParent = {
        x: 20 * this.scale,
        width: node.rect.width,
        height: node.rect.height,
        marginTop: 0,
        marginRight: 0,
        marginLeft: 0,
        rotate: 0,
      }
      
      const offHeight = hitNodeLen ? newNode.rect.height + 20 * this.scale : 100 / hitNode.level * this.scale  + 20 * this.scale
      if (hitNodeLen > 0) {
        // 如果有子节点
        const LasthitNodeChild = hitNode.children[hitNodeLen -1]
        newNode.rectInParent.y =  LasthitNodeChild.rectInParent.y + LasthitNodeChild.rectInParent.height + 20 * this.scale
      } else {
        // 如果没有子节点
        newNode.rectInParent.y =  offHeight
      }

      // 如果 hitNode 不是第一层，则长辈节点们都需要增加 offHeight，同时兄弟节点们都需要下移offHeight
      if (hitNode.level !== 1) {
        hitNode.children.push(newNode)

        let result = []
        // 寻找 node 的所有层级的 parent，所有parent 的节点高度都需要增加 offHeight ，其兄弟节点的都需要下移
        result = this.getAllParentsOfHitNode(this.curNodesList, hitNode.id)

        for(let i = 0; i < result.length; i++) {
          // 所有父代节点需要增加offHeight
          result[i].rect.height += offHeight
          result[i].rect.ey += offHeight
          result[i].rect.center.y = result[i].rect.y + 1/2 * result[i].rect.height
          this.calLineAnchorY(result[i], result[i].rect.center.y, 'centerY')

          // 最底层的父节点在最后，他没有兄弟节点，其他父代节点均需要其后的兄弟节点下移
          if ( i < result.length -1 ) {
            result[i].rectInParent.height += offHeight
            result[i].rectInParent.ey += offHeight
            // 找到父代节点所在的它的父节点children里的Index
            const index = result[i+1].children.findIndex(_node => _node.id === result[i].id)
            result[i+1].children.forEach((_node, _index) => {
              if ( _index > index) {
                _node.rectInParent.y += offHeight
                _node.rectInParent.ey += offHeight
                this.calBrotherLineY(_node, offHeight)
              }
            })
          }

          result[i].calcChildrenRect()
          result[i].init()
        }
      } else {

        // 如果是第一层
        hitNode.rect.height = hitNode.rect.height + offHeight
        hitNode.rect.ey = hitNode.rect.ey + offHeight
        hitNode.rect.center.y = hitNode.rect.y + 1/2 * hitNode.rect.height
        hitNode.children.push(newNode)
        hitNode.calcChildrenRect()
        this.calLineAnchorY(hitNode, hitNode.rect.center.y, 'centerY')
        hitNode.init()

      }
    },
    // 递归获取节点所有父级节点
    getAllParentsOfHitNode(List, id) {
      const result = []
      List.forEach(node => {
        if (node.id === id) {
          result.push(node)
        } else if(node.children?.length) {
          const list = this.getAllParentsOfHitNode(node.children, id)
          list.length && result.push(...list, node)
        }
      })
      return result
    },
    // 计算兄弟节点及其子节点的
    calBrotherLineY(node, offHeight) {
      this.calLineAnchorY(node, offHeight, 'offHeight')
      if (node.children?.length) {
        node.children.forEach(_node => this.calBrotherLineY(_node, offHeight))
      }
    },
    addLine(line) {
      // anchorIndex 0-左 1-上 2-右 3-下 x, y
      const isSameLinePoint = this.curLineList.slice(0, -1).some(_line => {
        return (_line.to.id === line.to.id && _line.to.anchorIndex === line.to.anchorIndex) ||
          (_line.to.id === line.from.id && _line.to.anchorIndex === line.from.anchorIndex) ||
          (_line.from.id === line.to.id && _line.from.anchorIndex === line.to.anchorIndex) ||
          (_line.from.id === line.from.id && _line.from.anchorIndex === line.from.anchorIndex)
        }
      )
      const isUpAndDownAnchorPoint = line.from.anchorIndex === 1 || 
                                    line.from.anchorIndex === 3 ||
                                    line.to.anchorIndex === 1 ||
                                    line.to.anchorIndex === 3
      // 中心点：??

      if (line.from.id === line.to.id) {
        this.deleteline('不可自身相连')
      } else if (isSameLinePoint) {
        this.deleteline('不可重复连点')
      } else if (isUpAndDownAnchorPoint) {
        this.deleteline('起止点不能是上下锚点')
      }
      
      // 增加线时左、下、右的线起始或者结束点增加offHeight
      // 如何确定是否连线
    },
    deleteline(message) {
      this.topology.delete()
      alert(message)
    },
    // 重绘连线锚点
    calLineAnchorY(node, h, mode) {
      this.curLineList.forEach(line => {
          if (line.to.id === node.id) {
            mode === 'centerY' ? line.to.y = h : line.to.y += h
            line.calcControlPoints()
          } else if (line.from.id === node.id) {
            mode === 'centerY' ? line.from.y = h : line.from.y += h
            line.calcControlPoints()
          }
        })
    },
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
  margin-top: 60px;
}
li {
  list-style: none;
  margin: 20px 10px;
}
.topology-demo {
  display: flex;
  flex-flow: row wrap;
}
</style>
