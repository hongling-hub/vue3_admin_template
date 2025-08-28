<template>
  <div class="box7">
    <div class="title">
      <p>年度游客量对比</p>
      <img src="../../images/dataScreen-title.png" alt="">
    </div>
    <div class="charts" ref="charts"></div>
  </div>
</template>

<script setup lang="ts">
import * as echarts from 'echarts';
import { ref, onMounted } from 'vue';
//获取DOM节点
let charts = ref();
//组件挂载完毕
onMounted(() => {
  //一个容器可以同时展示多种类型的图形图标
  let mychart = echarts.init(charts.value);
  //设置配置项
  mychart.setOption({
    tooltip: {
      trigger: 'axis',
      axisPointer: {
        type: 'cross',
        label: {
          backgroundColor: '#6a7985'
        }
      }
    },
    legend: {
      data: ['2021年', '2022年', '2023年'],
      textStyle: {
        color: 'white'
      },
      top: 10,
      // 设置图例靠右显示
      right: '10%',
      // 修改图例图标为矩形色块
      icon: 'rect',
      // 设置图例项宽度和高度
      itemWidth: 15,
      itemHeight: 15,
      // 设置图例项之间的间距
      itemGap: 20
    },
    grid: {
      left: '3%',
      right: '4%',
      bottom: '3%',
      containLabel: true
    },
    xAxis: [
      {
        type: 'category',
        boundaryGap: false,
        data: ['1月', '2月', '3月', '4月', '5月', '6月', '7月', '8月', '9月', '10月', '11月', '12月'],
        axisLine: {
          lineStyle: {
            color: 'rgba(255, 255, 255, 0.3)'
          }
        },
        axisLabel: {
          color: 'white'
        }
      }
    ],
    yAxis: [
      {
        type: 'value',
        name: '人数',
        nameTextStyle: {
          color: 'white'
        },
        max: 600,
        axisLine: {
          lineStyle: {
            color: 'rgba(255, 255, 255, 0.3)'
          }
        },
        axisLabel: {
          color: 'white',
          formatter: '{value}'
        },
        splitLine: {
          show: false
        }
      }
    ],
    series: [
      {
        name: '2021年',
        type: 'line',
        smooth: true,
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: 'rgba(234, 147, 7, 0.7)' },
            { offset: 1, color: 'rgba(234, 147, 7, 0.1)' }
          ])
        },
        emphasis: {
          focus: 'series'
        },
        lineStyle: {
          width: 1,
          color: '#ea9307'
        },
        itemStyle: {
          color: '#ea9307'
        },
        symbol: 'none',
        data: [180, 100, 150, 20, 60, 130, 120, 110, 100, 520, 430, 300]
      },
      {
        name: '2022年',
        type: 'line',
        smooth: true,
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: 'rgba(1, 122, 245, 0.7)' },
            { offset: 1, color: 'rgba(1, 122, 245, 0.1)' }
          ])
        },
        emphasis: {
          focus: 'series'
        },
        lineStyle: {
          width: 1,
          color: '#017af5'
        },
        itemStyle: {
          color: '#017af5'
        },
        symbol: 'none',
        data: [200, 500, 380, 250, 200, 320, 350, 200, 300, 480, 300, 130]
      },
      {
        name: '2023年',
        type: 'line',
        smooth: true,
        areaStyle: {
          color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [
            { offset: 0, color: 'rgba(250, 68, 2, 0.7)' },
            { offset: 1, color: 'rgba(250, 68, 2, 0.1)' }
          ])
        },
        emphasis: {
          focus: 'series'
        },
        lineStyle: {
          width: 1,
          color: '#fa4402'
        },
        itemStyle: {
          color: '#fa4402'
        },
        symbol: 'none',
        data: [520, 200, 150, 120, 100, 500, 50, 80, 60, 400, 200, 150]
      }
    ]
  })
})
</script>

<style scoped lang="scss">
.box7 {
  width: 100%;
  height: 100%;
  background: url(../../images/dataScreen-main-rc.png) no-repeat;
  background-size: 100% 100%;
  margin: 15px 0px;

  .title {
    p {
      color: white;
      font-size: 18px;
    }
  }

  .charts {
    height: calc(100% - 30px);
  }

}
</style>