<template>
  <div>
    <div>【服务资源占用比】</div>
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
      type: 'category',
      data: props.data.servers.map(item => item.name),
      axisLabel: {
        color: '#9EB1C8'
      }
    },
    // y轴展示数据
    yAxis: {
      show: false,
      type: 'value',
      max: function (value) {
        return parseInt(value.max * 1.2)
      }
    },
    // 绘制图标的位置
    grid: { // grid组件距离容器的距离
      top: 16,
      right: 0,
      bottom: 26,
      left: -26,
      containLabel: true // 决定是否包含最标轴标签在内的内容来形成矩形的位置
    },
    // 核心配置
    series: [
      {
        type: 'bar', // 指定类型为柱状图
        data: props.data.servers.map(item => ({
          name: item.name,
          value: item.value
        })), // 数据
        itemStyle: { // 图形样式
          color: '#479AD3 ', // 柱条颜色
          barBorderRadius: 5, // 圆角半径
          shadowColor: 'rgba(0, 0, 0, .3)', // 阴影颜色
          shadowBlur: 5 // 阴影模糊大小
        },
        barWidth: 12, // 柱条宽度
        label: { // 图形文本标签
          show: true,
          position: 'top', // 位置
          textStyle: {
            color: '#fff'
          },
          formatter: '{c}%'
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