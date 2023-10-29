<template>
  <div>
    <div>【大区数据信息】</div>
    <div ref="target" class="w-full h-full"></div>
  </div>
</template>

<script setup>
import { ref, onMounted, watch } from 'vue'
import * as echarts from 'echarts'
const props = defineProps({
  data: {
    type: Object,
    required: true
  }
})

// 1.初始化echart实例
let mChart = null
const target = ref(null)
onMounted(() => {
  mChart = echarts.init(target.value)
  renderChart()
})

// 2.构建options对象
const renderChart = () => {
  const options = {
    // x轴展示数据
    xAxis: {
      show: false, // 是否显示x轴
      type: 'value', // 数值轴，用于连续数据，默认'category'
      // 坐标轴刻度最大值，可以设置成'dataMax'(数据在轴上的最大值作为最大刻度)
      max: function (value) { // 防止数据触顶
        return parseInt(value.max * 1.2)
      }
    },
    // y轴展示数据
    yAxis: {
      type: 'category', // 类目轴，用于离散的类目数据，默认'value'
      data: props.data.regions.map(item => item.name), // 在类型为category中有效，设置y轴的取值范围
      inverse: true, // 是否反向坐标轴
      axisLine: { // 坐标轴轴线
        show: false
      },
      axisTick: { // 坐标轴刻度
        show: false
      },
      axisLabel: { // 坐标轴刻度标签
        color: '#9eb1c8'
      }
    },
    // 绘制图标的位置
    grid: { // grid组件距离容器的距离
      top: 0,
      right: 0,
      bottom: 0,
      left: 0,
      containLabel: true // 决定是否包含最标轴标签在内的内容来形成矩形的位置
    },
    // 核心配置
    series: [
      {
        type: 'bar', // 指定类型为柱状图
        data: props.data.regions.map(item => ({
          name: item.name,
          value: item.value
        })), // 数据
        showBackground: true,
        backgroundStyle: { // 展示柱条背景色
          color: 'rgba(180, 180, 180, .2)'
        },
        itemStyle: { // 图形样式
          color: '#479AD3 ', // 柱条颜色
          barBorderRadius: 5, // 圆角半径
          shadowColor: 'rgba(0, 0, 0, .3)', // 阴影颜色
          shadowBlur: 5 // 阴影模糊大小
        },
        barWidth: 12, // 柱条宽度
        label: { // 图形文本标签
          show: true,
          position: 'right', // 位置
          textStyle: {
            color: '#fff'
          }
        }
      }
    ]
  }

  // 3.设置options
  mChart.setOption(options)
}

watch(() => props.data, () => {
  renderChart()
})
</script>

<style></style>