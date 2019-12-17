<template>
  <div :class="className" :style="{ height: height, width: width }" />
</template>

<script>
import echarts from 'echarts'
require('echarts/theme/macarons') // echarts theme

const animationDuration = 3000

export default {
  props: {
    className: {
      type: String,
      default: 'chart'
    },
    width: {
      type: String,
      default: '100%'
    },
    height: {
      type: String,
      default: '300px'
    }
  },
  data() {
    return {
      chart: null
    }
  },
  mounted() {
    this.$nextTick(() => {
      this.initChart()
    })
  },
  beforeDestroy() {
    if (!this.chart) {
      return
    }
    this.chart.dispose()
    this.chart = null
  },
  methods: {
    initChart() {
      this.chart = echarts.init(this.$el, 'macarons')

      this.chart.setOption({
        tooltip: {
          trigger: 'axis',
          axisPointer: {
            // 坐标轴指示器，坐标轴触发有效
            type: 'shadow' // 默认为直线，可选为：'line' | 'shadow'
          }
        },
        radar: {
          radius: '66%',
          center: ['50%', '42%'],
          splitNumber: 8,
          splitArea: {
            areaStyle: {
              color: 'rgba(127,95,132,.3)',
              opacity: 1,
              shadowBlur: 45,
              shadowColor: 'rgba(0,0,0,.5)',
              shadowOffsetX: 0,
              shadowOffsetY: 15
            }
          },
          indicator: [
            { name: '武力', max: 100 },
            { name: '统帅', max: 100 },
            { name: '智力', max: 100 },
            { name: '政治', max: 100 },
            { name: '运气', max: 100 },
            { name: '忠诚', max: 100 }
          ]
        },
        legend: {
          left: 'center',
          bottom: '10',
          data: ['赵云', '诸葛亮', '廖化']
        },
        series: [
          {
            type: 'radar',
            symbolSize: 0,
            areaStyle: {
              normal: {
                shadowBlur: 13,
                shadowColor: 'rgba(0,0,0,.2)',
                shadowOffsetX: 0,
                shadowOffsetY: 10,
                opacity: 0.8
              }
            },
            data: [
              {
                value: [97, 92, 85, 72, 91, 82],
                name: '赵云'
              },
              {
                value: [56, 96, 99, 92, 88, 81],
                name: '诸葛亮'
              },
              {
                value: [78, 67, 64, 56, 94, 62],
                name: '廖化'
              }
            ],
            animationDuration: animationDuration
          }
        ]
      })
    }
  }
}
</script>
