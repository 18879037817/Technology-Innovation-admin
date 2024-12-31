<template>
  <div class="Scienceproject">
    <!-- 开始 -->

    <div class="Scienceproject-box">
      <div>
        <div class="s-bottom">
          <div class="s-title">科创项目管理模块</div>
        </div>

        <ul class="flex flex-wrap flex-space sclice-pad">
          <li v-for="(item, index) in Scienceproject" class="Scienceproject-li">
            <div class="line Scienceproject-item">
              <ul class="flex box-title">
                <li class="title-items">{{ item.title }}</li>
                <li>
                  <el-icon style="vertical-align: text-bottom;" color="#ccc">
                    <SuccessFilled />
                  </el-icon>
                </li>
              </ul>
            </div>

            <div class="Scienceproject-pad">
              <ul class="flex Scienceproject-bottom">
                <li>
                  <div class="title-items">1111</div>
                  <div class="title-items">总条数</div>
                </li>
                <li>
                  <div>
                    <!-- <el-icon style="vertical-align: text-bottom;">
                      <Aim />
                    </el-icon> -->

                    <el-icon style="vertical-align: text-bottom;" color="#ccc">
                      <SuccessFilled />
                    </el-icon>
                  </div>
                  <div class="title-items">5</div>
                </li>
              </ul>
            </div>
          </li>
        </ul>
      </div>
    </div>
    <!-- 结束 -->
  </div>
</template>


<script setup lang="ts">

defineOptions({
  name: "Scienceproject",
  inheritAttrs: false,
})
const Scienceproject = ref([
  {
    title: '需求上报',
    icon: 'el-icon-help',
    total: '222',
    userTotal: '333'
  },
  {
    title: '需求上报',
    icon: 'el-icon-help',
    total: '222',
    userTotal: '333'
  },
  {
    title: '需求上报',
    icon: 'el-icon-help',
    total: '222',
    userTotal: '333'
  },
  {
    title: '需求上报',
    icon: 'el-icon-help',
    total: '222',
    userTotal: '333'
  }
])



onMounted(() => {



});

</script>

<style lang="scss" scoped>
.Scienceproject {
  margin-top: 20px;

  // box-shadow: 3px 3px 10px #e0e0e0;
  // border-radius: 5px;
  // background-color: #ffffff;
  // padding: 20px;
  .flex-space {
    justify-content: space-between;

    .Scienceproject-li {
      background-color: #fff;
      box-shadow: 3px 3px 10px #e0e0e0;
      border-radius: 5px;
      width: 49%;
      margin-bottom: 13px;
    }
  }

  .sclice-pad {
    padding: 20px;
  }


  .line {
    border-bottom: 1px solid #ccc;

    .box-title {
      padding: 10px 10px;
      align-items: center;
      justify-content: space-between;


    }
  }

  .Scienceproject-box {
    flex-wrap: wrap;
    height: 340px;
    // width: 500px;
    // padding: 10px 20px;
    background-color: #fff;
    justify-content: space-between;

  }


  .Scienceproject-item {}

  .Scienceproject-pad {
    padding: 10px 10px;
  }

  .Scienceproject-bottom {
    justify-content: space-between;
  }

  .s-bottom {
    border-bottom: 1px solid #e4e7ed;
  }

  .s-title {
    color: #6d6c6c;
    font-weight: bold;
    padding: 12px 15px;
    // margin-bottom: 20px;
  }

  .title-items {
    color: #757272;
  }
}
</style>