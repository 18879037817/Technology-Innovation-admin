<template>
  <div class="Innovationvitality">
    <div>
      <div>积分排行榜</div>
    </div>
    <!-- -->
    <div class="Innovationvitality-box">
      <div class="Innovationvitality-h3">
        <h3 class="h3_color">{{ Title }}</h3>
      </div>
      <div class="flex flex-x-around">
        <div class="text-align">
          <img src="@/assets/icons/number_1.svg" alt="">
          <div>
            <p>会昌分公司</p>
            <p>20</p>
          </div>
        </div>
        <div class="text-align">
          <img src="@/assets/icons/number_2.svg" alt="">
          <div>
            <p>赣县分公司</p>
            <p>20</p>
          </div>
        </div>
        <div class="text-align">
          <img src="@/assets/icons/number_3.svg" alt="">
          <div>
            <p>经济技术开发区分公司</p>
            <p>20</p>
          </div>
        </div>
      </div>
      <div>

        <el-table ref="dataTableRef" v-loading="loading" :data="roleList" highlight-current-row>
          <el-table-column label="分公司名称" prop="branchoffice" />
          <el-table-column label="数量" prop="number" />
        </el-table>
      </div>
    </div>
  </div>
</template>
<script setup lang="ts">
defineOptions({
  name: "Notice",
  inheritAttrs: false,
});
const roleList = ref([
  {
    branchoffice: '龙南市分公司',
    number: '20'
  },
  {
    branchoffice: '南康区分公司',
    number: '20'
  },
  {
    branchoffice: '赣县公司',
    number: '20'
  },
  {
    branchoffice: '经济技术开发区分公司',
    number: '20'
  },
  {
    branchoffice: '龙南市分公司',
    number: '20'
  }
])
const loading = ref(false);
const Title = ref('');
</script>

<style lang="scss" scoped>
.Innovationvitality {
  box-shadow: 3px 3px 10px #e0e0e0;
  border-radius: 5px;
  background-color: #ffffff;
  padding: 20px;
  height: 440px;
  overflow-y: auto;

  .Innovationvitality-box {
    .flex-x-around {
      justify-content: space-around;
    }

    .Innovationvitality-h3 {
      .h3_color {
        color: #929292;
      }
    }

    .text-align {
      text-align: center;
    }

    .text-align p {
      padding: 0px;
      margin: 8px 0px;
    }
  }

}
</style>
