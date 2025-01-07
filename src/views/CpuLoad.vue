<template>
  <a-space wrap>
    <div :id="`cpu-${i}`" style="width: 180px;height: 180px;border: 1px solid black;" v-for="i in 16"></div>
  </a-space>
</template>

<script setup>
import * as echarts from 'echarts';
import { onMounted, ref } from 'vue';
import { getCpuLoadApi } from '@/api/cpuLoadApi';

onMounted(() => {
  initChart();
  getAndDrawCpuLoad();
});

const cpuData = ref([]);

const chartDoms = [];

const initChart = () => {
  for (let i = 1; i <= 16; i++) {
    var chartDom = document.getElementById('cpu-' + i);
    chartDoms.push(echarts.init(chartDom));
  }
}

const getAndDrawCpuLoad = async () => {
  let res = await getCpuLoadApi();

  let data = res.data;

  for (let i = 1; i <= 16; i++) {
    if (!cpuData.value[i - 1]) {
      cpuData.value[i - 1] = [];
    }
    cpuData.value[i - 1].push(data[i - 1]);
    if (cpuData.value[i - 1].length > 10) { // 保留最近10个数据
      cpuData.value[i - 1].shift();
    }
    drawCpuLoad(i, cpuData.value[i - 1]);
  }
  setTimeout(() => {
    getAndDrawCpuLoad();
  }, 1000);
}

const drawCpuLoad = (cpuIndex, cpuData) => {
  var option;
  option = {
    title: {
      text: 'CPU-' + cpuIndex,
      textStyle: {
        fontSize: 14
        }
    },
    grid: {
      top: 0,
      bottom: 5,
      left: 0,
      right: 0
    },
    xAxis: {
      show: false,
      type: 'category'
    },
    yAxis: {
      show: false,
      type: 'value',
      min: 0,
      max: 1
    },
    series: [
    {
      data: cpuData,
      type: 'line',
      areaStyle: {},
      smooth: true,
      symbol: 'none'
    }
    ]
  };
  chartDoms[cpuIndex - 1].setOption(option);
}

</script>

<style scoped>
</style>