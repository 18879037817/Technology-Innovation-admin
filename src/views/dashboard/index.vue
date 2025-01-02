<template>
  <div class="dashboard-container">
    <!-- 单位科创指数排行榜 -->
    <div class="flex home-top">
      <div class="flex-row">
        <Innovationvitality></Innovationvitality>
      </div>
      <div class="flex-row">
        <Notice></Notice>
      </div>
      <div class="flex-row">
        <Companyecharts></Companyecharts>
      </div>
    </div>

    <div class="flex home-bottom">
      <div>
        <Scienceproject></Scienceproject>
      </div>
      <div>
        <ExpertTeammanagement></ExpertTeammanagement>
      </div>
    </div>


    <div class="flex home-bottom">
      <div>
        <Excellentachievements></Excellentachievements>
      </div>
      <div>
        <Innovativefax></Innovativefax>
      </div>
    </div>
    <ganttchart ref="ganttChartRef"></ganttchart>
  </div>
</template>

<script setup lang="ts">

defineOptions({
  name: "Dashboard",
  inheritAttrs: false,
});
import Innovationvitality from "./components/Innovationvitality.vue"
import Notice from "./components/Notice.vue"
import Companyecharts from "./components/Companyecharts.vue"
import Scienceproject from "./components/Scienceproject.vue"
import ExpertTeammanagement from "./components/ExpertTeammanagement.vue"
import Excellentachievements from "./components/Excellentachievements.vue"
import Innovativefax from "./components/Innovativefax.vue"
import ganttchart from "./components/ganttchart.vue"
onMounted(() => {

});
</script>

<style lang="scss" scoped>
.dashboard-container {
  position: relative;
  padding: 24px;

  .home-top {
    justify-content: space-between;

    .flex-row {
      width: 32%;
    }
  }

  .home-bottom>div {
    width: 50%;
  }
}
</style>
