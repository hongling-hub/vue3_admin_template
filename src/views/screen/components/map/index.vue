<template>
    <div class="box4">
        <div class="title">
            <p>景区实时客流量</p>
            <img src="../../images/dataScreen-title.png" alt="">
        </div>
        <div class="map-container" ref="map"></div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted, nextTick } from 'vue';
import * as echarts from 'echarts';
//引入中国地图的JSON数据
import chinaJSON from './china.json'
//获取DOM元素
let map = ref();
//注册中国地图
 echarts.registerMap('china', chinaJSON as any)
onMounted(() => {
    nextTick(() => {
        let mychart = echarts.init(map.value);
        //设置配置项
        mychart.setOption({
        // 标题
            title: {
                text: '景区实时客流量',
                left: '10%',
                top: '5%',
                textStyle: {
                    color: '#fff',
                    fontSize: 18,
                    fontWeight: 'bold'
                }
            },
            // 提示框
            tooltip: {
                trigger: 'item',
                formatter: '{b}: {c}'
            },
            //地图组件
            geo: {
                map: 'china',//中国地图
                roam: true,//鼠标缩放的效果
                left: '5%',
                top: '15%',
                right: '5%',
                bottom: '5%',
                zoom: 1.2,
                //地图上的文字的设置
                label: {
                    show: true,
                    color: '#fff',
                    fontSize: 12
                },
                itemStyle: {
                    // 地图区域的样式
                    color: '#152E5F',
                    borderColor: '#1E4D91',
                    borderWidth: 1,
                    areaStyle: {
                        color: new echarts.graphic.RadialGradient(0.5, 0.5, 1, [
                            { offset: 0, color: '#0F3B77' },
                            { offset: 1, color: '#071F3D' }
                        ])
                    }
                },
                //地图高亮的效果
                emphasis: {
                    itemStyle: {
                        color: '#2B58A1'
                    },
                    label: {
                        color: '#fff',
                        fontSize: 14
                    }
                }
            },
            series: [
                // 城市节点
                {
                    type: 'scatter',
                    coordinateSystem: 'geo',
                    symbolSize: 10,
                    data: [
                        { name: '北京', value: [116.405285, 39.904989], itemStyle: { color: '#FF7A45' } },
                        { name: '上海', value: [121.473701, 31.230416], itemStyle: { color: '#FF7A45' } },
                        { name: '广州', value: [113.264385, 23.12911], itemStyle: { color: '#FF7A45' } },
                        { name: '深圳', value: [114.057868, 22.543096], itemStyle: { color: '#FF7A45' } },
                        { name: '成都', value: [104.066541, 30.572269], itemStyle: { color: '#FF7A45' } },
                        { name: '杭州', value: [120.15507, 30.274084], itemStyle: { color: '#FF7A45' } },
                        { name: '武汉', value: [114.305551, 30.592855], itemStyle: { color: '#FF7A45' } },
                        { name: '西安', value: [108.948024, 34.263161], itemStyle: { color: '#FF7A45' } },
                        { name: '新疆', value: [87.617733, 43.792818], itemStyle: { color: '#FF7A45' } }

                    ],
                    emphasis: {
                        symbolSize: 15
                    }
                },
                // 航线
                {
                    type: 'lines',
                    coordinateSystem: 'geo',
                    zlevel: 2,
                    effect: {
                        show: true,
                        symbol: `image://${new URL('@/views/screen/images/plane.svg', import.meta.url).href}`,
                         symbolSize: 16,
                         trailLength: 0
                    },
                    lineStyle: {
                        color: new echarts.graphic.LinearGradient(0, 0, 1, 0, [
                            { offset: 0, color: '#36CFC9' },
                            { offset: 1, color: '#1890FF' }
                        ]),
                        width: 2,
                        type: 'dashed',
                        opacity: 0.6
                    },
                    data: [
                        { coords: [[116.405285, 39.904989], [121.473701, 31.230416]] },
                        { coords: [[116.405285, 39.904989], [113.264385, 23.12911]] },
                        { coords: [[121.473701, 31.230416], [113.264385, 23.12911]] },
                        { coords: [[113.264385, 23.12911], [114.057868, 22.543096]] },
                        { coords: [[104.065735, 30.659462], [87.617733, 43.792818]] },
                        { coords: [[120.15507, 30.274084], [114.305551, 30.592855]] },
                        { coords: [[108.948024, 34.263161], [116.405285, 39.904989]] }
                    ]
                }
            ]
    });
        window.addEventListener('resize', () => {
            mychart.resize();
        });
    });

});
</script>

<style scoped lang="scss">
.box4 {
    width: 100%;
    height: 100%;
   
   
    .title {
        position: absolute;
        top: 10px;
        left: 20px;
        z-index: 10;

        p {
            color: white;
            font-size: 18px;
            font-weight: bold;
        }
    }

    .map-container {
        width: 100%;
        height: 100%;
    }
}
</style>