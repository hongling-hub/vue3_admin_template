<template>
    <div class="box8">
        <div class="title">
            <p>预约渠道数据统计</p>
            <img src="../../images/dataScreen-title.png" alt="">
        </div>
        <div class="charts" ref="charts"></div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';
import * as echarts from 'echarts';
let charts = ref();

onMounted(() => {
    nextTick(() => {
        let mychart = echarts.init(charts.value);
        let option = {
            tooltip: {
                trigger: 'item',
                formatter: '{b}: {d}%'
            },
            legend: {
                orient: 'vertical',
                left: 20,
                top: 'center',
                textStyle: {
                    color: 'white',
                    fontSize: 14
                },
                icon: 'circle'
            },
            graphic: [
                {
                    type: 'circle',
                    z: 100,
                    cx: '50%',
                    cy: '50%',
                    r: 60,
                    style: {
                        fill: 'transparent',
                        stroke: '#0b2f57',
                        lineWidth: 2
                    }
                }
            ],
            // 移除了path类型的图形元素，因为它导致了错误
            series: [
                {
                    name: '预约渠道',
                    type: 'pie',
                    radius: ['40%', '70%'],
                    center: ['50%', '50%'],
                    avoidLabelOverlap: false,
                    itemStyle: {
                        borderRadius: 10,
                        borderColor: '#0f2544',
                        borderWidth: 2,
                        shadowBlur: 10,
                        shadowOffsetX: 0,
                        shadowColor: 'rgba(0, 0, 0, 0.5)'
                    },
                    label: {
                        show: true,
                        position: 'outside',
                        color: '#fff',
                        fontSize: 12,
                        formatter: '{b}: {d}%',
                        distance: 15
                    },
                    emphasis: {
                        itemStyle: {
                            shadowBlur: 20,
                            shadowOffsetX: 0,
                            shadowColor: 'rgba(0, 0, 0, 0.8)'
                        },
                        label: {
                            show: true,
                            fontSize: 16,
                            fontWeight: 'bold'
                        }
                    },
                    labelLine: {
                        show: true,
                        length: 20,
                        length2: 30,
                        lineStyle: {
                            color: '#fff',
                            width: 1
                        }
                    },
                    animationType: 'scale',
                    animationEasing: 'elasticOut',
                    animationDelay: function (idx:any) {
                        return idx * 100;
                    },
                    data: [
                        { value: 40, name: '智慧文旅平台', itemStyle: { color: '#36CFC9' } },
                        { value: 10, name: '抖音', itemStyle: { color: '#FF7A45' } },
                        { value: 20, name: '飞猪', itemStyle: { color: '#722ED1' } },
                        { value: 30, name: '其他渠道', itemStyle: { color: '#F5222D' } }
                    ]
                }
            ]
        };
        mychart.setOption(option);
        window.addEventListener('resize', () => {
            mychart.resize();
        });
    });
});
</script>

<style scoped lang="scss">
.box8 {
    width: 100%;
    height: 100%;
    background: url(../../images/dataScreen-main-rb.png) no-repeat;
    background-size: 100% 100%;

    .title {
        margin-left: 20px;
        padding-top: 10px;

        p {
            color: white;
            font-size: 18px;
        }
    }

    .charts {
        width: 100%;
        height: calc(100% - 40px);
        
    }
}
</style>