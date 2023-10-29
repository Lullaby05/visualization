<template>
  <div>
    <div>【云端报警风险】</div>
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
    radar: {
      name: {
        textStyle: {
          color: '#05D5FF',
          fontSize: 14 // label名字
        }
      },
      shape: 'polygon',
      center: ['50%', '50%'],
      radius: '80%',
      startAngle: 120, // 起始旋转角度
      axisLine: {
        lineStyle: {
          color: 'rgba(5, 213, 255, .8)'
        }
      },
      splitLine: {
        show: true,
        lineStyle: {
          width: 1,
          color: 'rgba(5, 213, 255, .8)'
        }
      },
      indicator: props.data.risks.map(item => ({
        name: item.name,
        max: 100
      })),
    },
    polar: {
      center: ['50%', '50%'],
      radius: '0%'
    },
    angleAxis: {
      min: 0,
      interval: 5,
      clockwise: false
    },
    radiusAxis: {
      min: 0,
      interval: 20,
      splitLine: {
        show: true
      }
    },
    series: [{
      type: 'radar',
      symbol: 'circle',
      symbolSize: 10,
      itemStyle: {
        normal: {
          color: '#05D5FF'
        }
      },
      areaStyle: {
        normal: {
          color: '#05D5FF',
          opacity: 0.5
        }
      },
      lineStyle: {
        width: 2,
        color: '#05D5FF'
      },
      label: {
        normal: {
          show: true,
          color: '#fff'
        }
      },
      data: [{ value: props.data.risks.map(item => item.value) }]
    }]
  }

  // 3.设置options
  mChart.setOption(options)
}

watch(() => props.data, () => {
  renderChart()
})
</script>

<style></style>