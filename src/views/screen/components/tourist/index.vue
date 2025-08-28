<template>
  <div class="box">
    <div class="top">
      <p class="title">实时游客统计</p>
      <p class="bg"></p>
      <p class="right">
        可预约总量
        <span>99999</span>
        人
      </p>
    </div>
    <div class="number">
      <span v-for="(item, index) in people" :key="index">{{ item }}</span>
    </div>
    <!-- 盒子将来echarts展示图形图标的节点 -->
    <div class="charts" ref="charts">123</div>
  </div>
</template>

<script setup lang="ts">
import 'echarts-liquidfill'
import * as echarts from 'echarts'
import { ref, onMounted } from 'vue'
let people = ref('215908人')

//水球图拓展插件

//获取节点
let charts = ref()
onMounted(() => {
  //获取echarts类的实例
  let mychart = echarts.init(charts.value)
  //设置实例的配置项
  const value = 0.5
  let option = {
    backgroundColor: '#0F224C', //背景色
    series: [
      {
        name: '预估量',
        type: 'liquidFill',
        radius: '90%',
        center: ['50%', '50%'],
        backgroundStyle: {
          color: 'transparent',
        },
        data: [value, value],
        amplitude: 20, //水波振幅
        label: {
          //标签设置
          position: ['50%', '45%'],
          formatter: '预测量', //显示文本,
          textStyle: {
            fontSize: '20px', //文本字号,
            color: '#fff',
          },
        },
        outline: {
          borderDistance: 3,
          itemStyle: {
            borderWidth: 2,
            borderColor: {
              type: 'linear',
              x: 1,
              y: 0,
              x2: 0,
              y2: 0,
              colorStops: [
                {
                  offset: 0,
                  color: '#007DFF',
                },
                {
                  offset: 0.6,
                  color: 'rgba(45, 67, 114, 1)',
                },
                {
                  offset: 1,
                  color: 'rgba(45, 67, 114, 1)',
                },
              ],
              globalCoord: false,
            },
          },
        },
        itemStyle: {
          color: new echarts.graphic.LinearGradient(0, 1, 0, 0, [
            {
              offset: 1,
              color: 'rgba(31, 222, 225, 1)',
            },
            {
              offset: 0.85,
              color: '#007DFF80',
            },
            {
              offset: 0.35,
              color: '#004a99',
            },
            {
              offset: 0,
              color: 'rgba(31, 222, 225, 1)',
            },
          ]),
        },
      },
    ],
  }
  //设置配置项
  mychart.setOption(option)
})
</script>

<style scoped lang="scss">
.box {
  background: url(../../images/dataScreen-main-lb.png) no-repeat;
  background-size: 100% 100%;
  margin-top: 10px;

  .top {
    margin-left: 20px;

    .title {
      color: white;
      font-size: 20px;
    }

    .bg {
      width: 68px;
      height: 7px;
      background: url(../../images/dataScreen-title.png) no-repeat;
      background-size: 100% 100%;
      margin-top: 10px;
    }

    .right {
      float: right;
      color: white;
      font-size: 20px;

      span {
        color: orange;
        font-style: italic;
      }
    }
  }

  .number {
    padding: 10px;
    margin-top: 30px;
    display: flex;

    span {
      flex: 1;
      height: 40px;
      text-align: center;
      line-height: 40px;
      background: url(../../images/total.png) no-repeat;
      background-size: 100% 100%;
      color: #29fcff;
    }
  }

  .charts {
    width: 100%;
    height: 270px;
}
}
</style>