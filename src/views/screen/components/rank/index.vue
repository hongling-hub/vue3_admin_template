<template>
    <div class="box6">
        <div class="title">
            <p>热门景区排行</p>
            <img src="../../images/dataScreen-title.png" alt="">
        </div>
        <!-- 图形图标的容器 -->
        <div class="charts" ref='charts'></div>
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
    function randomNum(min: number, max: number) {
        return Math.floor(Math.random() * (max - min) + min);
    }
    function roundDown(num: number) {
        const n = Math.floor(Math.log10(Number(num)));
        return Math.floor(num / Math.pow(10, n)) * Math.pow(10, n);
    }
    // 景区数据
    let scenicData = [
        { value: 80000, name: '峨眉山' },
        { value: 60000, name: '稻城亚丁' },
        { value: 50000, name: '九寨沟' },
        { value: 40000, name: '万里长城' },
        { value: 30000, name: '北京故宫' }
    ];
    let xAxisData = scenicData.map(item => item.name);
    let seriesData = scenicData.map(item => item.value);
    let maxSeriesData = new Array(scenicData.length).fill(roundDown(scenicData[0].value) * 2);
    let barLinearColors = [
        new echarts.graphic.LinearGradient(0, 0, 1, 0, [
            { offset: 0, color: "#2196F3" },
            { offset: 1, color: "#21CBF3" }
        ]),
        new echarts.graphic.LinearGradient(0, 0, 1, 0, [
            { offset: 0, color: "#FF5722" },
            { offset: 1, color: "#FF7A45" }
        ]),
        new echarts.graphic.LinearGradient(0, 0, 1, 0, [
            { offset: 0, color: "#03A9F4" },
            { offset: 1, color: "#03E5F4" }
        ]),
        new echarts.graphic.LinearGradient(0, 0, 1, 0, [
            { offset: 0, color: "#FFC107" },
            { offset: 1, color: "#FFEA00" }
        ]),
        new echarts.graphic.LinearGradient(0, 0, 1, 0, [
            { offset: 0, color: "#9C27B0" },
            { offset: 1, color: "#E040FB" }
        ])
    ];
    function rankBarColor(cData: number[]) {
        let tempData: any[] = [];
        cData.forEach((item, index) => {
            tempData.push({
                value: item,
                itemStyle: {
                    color: barLinearColors[index % barLinearColors.length]
                }
            });
        });
        return tempData;
    }
    let option = {
        //标题组件
        // title: {
        //     text: '热门景区排行',
        //     left: 'center',
        //     textStyle: {
        //         color: '#fff',
        //         fontSize: 18
        //     }
        // },
        tooltip: {
            trigger: 'axis',
            axisPointer: {
                type: 'shadow'
            },
            formatter: function (params: any) {
                return params && params[0].marker + params[0].name + ': ' + (params[0].value / 10000).toFixed(2) + 'w';
            }
        },
        grid: { left: '30%', top: '10%', bottom: '5%', right: '5%' },
        xAxis: {
            type: "value",
            splitLine: { show: false },
            axisLabel: { 
                show:false
            },
            axisTick: { show: false },
            axisLine: { 
                show: false
            }
        },
        yAxis: [
            {
                type: "category",
                inverse: true,
                axisLine: { show: false },
                axisTick: { show: false },
                data: xAxisData,
                axisLabel: {
                    rich: {
                        // 冠军样式
                        nt1: {
                            color: '#fff',
                            backgroundColor: '#FFD700',
                            width: 30,
                            height: 30,
                            fontSize: 16,
                            fontWeight: 'bold',
                            align: 'center',
                            borderRadius: 100,
                            padding: [0, 0, 0, 0]
                        },
                        // 亚军样式
                        nt2: {
                            color: '#fff',
                            backgroundColor: '#C0C0C0',
                            width: 30,
                            height: 30,
                            fontSize: 16,
                            fontWeight: 'bold',
                            align: 'center',
                            borderRadius: 100,
                            padding: [0, 0, 0, 0]
                        },
                        // 季军样式
                        nt3: {
                            color: '#fff',
                            backgroundColor: '#CD7F32',
                            width: 30,
                            height: 30,
                            fontSize: 16,
                            fontWeight: 'bold',
                            align: 'center',
                            borderRadius: 100,
                            padding: [0, 0, 0, 0]
                        },
                        // 其他排名样式
                        nt: {
                            color: '#fff',
                            backgroundColor: '#2196F3',
                            width: 30,
                            height: 30,
                            fontSize: 16,
                            fontWeight: 'bold',
                            align: 'center',
                            borderRadius: 100,
                            padding: [0, 0, 0, 0]
                        },
                        nameText: {
                            color: '#fff',
                            fontSize: 14,
                            width: 80,
                            align: 'left',
                            padding: [0, 0, 0, 10]
                        }
                    },
                    formatter: function (value: string) {
                        let idx = xAxisData.indexOf(value);
                        let rank = idx + 1;
                        // 根据排名选择不同样式
                        let style = 'nt';
                        if (rank === 1) style = 'nt1';
                        else if (rank === 2) style = 'nt2';
                        else if (rank === 3) style = 'nt3';
                        return [`{${style}|${rank}}`, `{nameText|${value}}`].join('');
                    }
                }
            }
        ],
        series: [
            {
                zlevel: 1,
                type: "bar",
                barWidth: 20,
                data: rankBarColor(seriesData),
                itemStyle: {
                    normal: {
                        barBorderRadius: [0, 5, 5, 0]
                    }
                },
                label: {
                    show: true,
                    position: 'right',
                    fontSize: 12,
                    color: "#fff",
                    formatter: function(params: any) {
                        // 计算百分比
                        const total = seriesData.reduce((sum, val) => sum + val, 0);
                        const percentage = Math.round((params.value / total) * 100);
                        return `${percentage}%  ${(params.value / 10000).toFixed(2)}w`;
                    }
                }
            },
            {
                type: "bar",
                barWidth: 20,
                barGap: "-100%",
                itemStyle: {
                    normal: {
                        barBorderRadius: [0, 5, 5, 0],
                        color: 'rgba(0,0,0,0.1)'
                    }
                },
                data: maxSeriesData
            }
        ]
    };
    mychart.setOption(option);
    // 响应式调整
    window.addEventListener('resize', () => {
        mychart.resize();
    });
});
</script>


<style scoped lang="scss">
.box6 {
    width: 100%;
    height: 100%;
    background: url(../../images/dataScreen-main-rt.png) no-repeat;
    background-size: 100% 100%;
    padding: 10px;

    .title {
        margin-bottom: 10px;

        p {
            color: white;
            font-size: 20px;
        }
    }

    .charts {
        height: calc(100% - 30px);
    }
}
</style>