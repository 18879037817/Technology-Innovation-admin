<template>
  <div class="Companyecharts">
    <!-- <div>
      <h3>各单位金点子排行榜</h3>
    </div> -->
    <div id="main" style="width: 100%;height:400px;"></div>
  </div>
</template>

<script setup lang="ts">
import * as echarts from "echarts";
defineOptions({
  name: "Companyecharts",
  inheritAttrs: false,
});
onMounted(() => {
  getoptionData();
});
function getoptionData() {
  // 基于准备好的dom，初始化echarts实例
  var myChart = echarts.init(document.getElementById('main'));

  // 指定图表的配置项和数据
  var option = {
    // 设置图表标题
    title: {
      text: '各单位金点子排行榜'
    },
    // 图表是纵向的
    grid: {
      left: '3%',
      right: '4%',
      bottom: '3%',
      containLabel: true
    },
    // 设置x轴
    xAxis: {
      type: 'value'
    },
    // 设置y轴
    yAxis: {
      type: 'category',
      data: ['南昌分公司', '赣县分公司', '南康分公司', '经开区分公司', '信丰分公司']
    },
    // 设置系列列表
    series: [
      {
        // 指定图表类型
        type: 'bar',
        // 设置数据
        data: [10, 20, 30, 40, 50],
        // 设置为纵向
        barWidth: '30%',
        // 设置柱子的颜色
        itemStyle: {
          color: '#1f78b4'
        }
      }
    ]
  };

  // 使用刚指定的配置项和数据显示图表。
  myChart.setOption(option);
}
</script>

<style lang="scss" scoped>
.Companyecharts {
  box-shadow: 3px 3px 10px #e0e0e0;
  border-radius: 5px;
  background-color: #ffffff;
  padding: 20px;
}
</style>
