<template>
    <div class="box2">
        <div class="title">
            <p>年龄比例</p>
            <img src="../../images/dataScreen-title.png" alt="">
        </div>
        <!-- 图形图标的容器 -->
        <div class="charts" ref="charts"></div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';
//引入echarts
import * as echarts from 'echarts';
let charts = ref();
//组件挂载完毕初始化图形图标
onMounted(() => {
    nextTick(() => {
        let mychart = echarts.init(charts.value);
        //设置配置项
        let option = {
            tooltip: {
                trigger: 'item',
                formatter: '{b}: {d}%'
            },
            legend: {
                right: 10,
                top: 'center',
                orient: 'vertical',//图例组件方向的设置
                textStyle: {
                    color: 'white',
                    fontSize: 12
                }
            },
            series: [
                {
                    name: '年龄比例',
                    type: 'pie',
                    radius: ['30%', '75%'],
                    center: ['40%', '50%'],
                    avoidLabelOverlap: false,
                    itemStyle: {
                        borderRadius: 10,
                        borderColor: '#fff',
                        borderWidth: 1
                    },
                    label: {
                        show: true,
                        position: 'inside',
                        color:'white',
                        formatter: '{d}%'
                    },
                    labelLine: {
                        show: false
                    },
                    data: [
                        { value: 16, name: '10岁以下', itemStyle: { color: '#1E90FF' } },
                        { value: 8, name: '10-18岁', itemStyle: { color: '#32CD32' } },
                        { value: 12, name: '18-30岁', itemStyle: { color: '#FFD700' } },
                        { value: 24, name: '30-40岁', itemStyle: { color: '#FF6347' } },
                        { value: 20, name: '40-60岁', itemStyle: { color: '#4169E1' } },
                        { value: 20, name: '60岁以上', itemStyle: { color: '#2E8B57' } }
                    ]
                }
            ]
        };
        mychart.setOption(option);
        // 响应窗口大小变化
        window.addEventListener('resize', () => {
            mychart.resize();
        });
    });
});
</script>

<style scoped lang="scss">
.box2 {
    width: 100%;
    height: 100%;
    background: url(../../images/dataScreen-main-cb.png) no-repeat;
    background-size: 100% 100%;
    position: relative;

    .title {
        margin-left: 20px;
        padding-top: 10px;

        p {
            color: white;
            font-size: 20px;
        }
    }

    .charts {
        width: 100%;
        height: calc(100% - 40px);
        position: absolute;
        top: 40px;
        left: 0;
    }

}
</style>