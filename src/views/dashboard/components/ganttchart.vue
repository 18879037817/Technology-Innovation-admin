<template>
  <div class="ganttchart">
    <!-- <div>
      <h3>各单位金点子排行榜</h3>
    </div> -->
    <div ref="progressChart" class="progressChart"></div>
  </div>
</template>

<script setup lang="ts">
import * as echarts from "echarts";
defineOptions({
  name: "ganttchart",
  inheritAttrs: false,
});
const progressChart = ref(null);
onMounted(() => {
  initChart();
});
function initChart() {
  const chart = echarts.init(progressChart.value);
  const option = {
    tooltip: {
      trigger: 'axis',
      formatter(params: any) {
        if (params[1].data && params[0].data) {
          return `<div>开始时间：${params[1].data}</div>` + `<div>结束时间：${params[0].data}</div>`;
        } else {
          return '';
        }
      },
      axisPointer: {
        type: 'shadow'
      }
    },
    grid: {
      containLabel: true,
      show: false,
      right: 80,
      left: 40,
      bottom: 40,
      top: 20,
      backgroundColor: '#fff'
    },
    legend: {
      data: ['持续时间'],
      align: 'auto',
      top: 'bottom'
    },
    xAxis: {
      type: 'time',
      position: 'top',
      axisTick: {
        show: false
      },
      axisLine: {
        show: false
      },
      splitLine: {
        show: true
      }
    },
    yAxis: {
      inverse: true,
      axisTick: {
        show: false
      },
      axisLine: {
        show: false
      },
      data: ['项目一', '项目二', '项目三', '项目四', '项目五', '项目六', '项目七']
    },
    series: [
      {
        name: '持续时间',
        type: 'bar',
        stack: 'duration',
        itemStyle: {
          color: '#007acc',
          borderColor: '#fff',
          borderWidth: 1
        },
        zlevel: -1,
        data: ['2021-01-31', '2021-02-25', '2021-03-25', '2021-04-01', '2021-04-10', '2021-05-25', '2021-07-25'] // 结束时间
      },
      {
        name: '持续时间',
        type: 'bar',
        stack: 'duration',
        itemStyle: {
          color: '#fff'
        },
        zlevel: -1,
        z: 9,
        data: ['2021-01-01', '2021-01-31', '2021-02-25', '2021-03-25', '2021-04-01', '2021-04-10', '2021-05-25'] // 开始时间
      }
    ]
  };
  chart.setOption(option);
  window.addEventListener('resize', () => {
    chart.resize();
  });
}
</script>

<style lang="scss" scoped>
.ganttchart {
  box-shadow: 3px 3px 10px #e0e0e0;
  border-radius: 5px;
  background-color: #ffffff;
  padding: 20px;
}

.progressChart {
  width: 80%;
  height: 500px;
  border: 1px solid #aaa;
}
</style>
