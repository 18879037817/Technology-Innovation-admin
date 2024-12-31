<template>
  <div class="Excellentachievements">
    <div class="ExpertTeammanagement-bc">
      <div class="h3_pad">
        <div class="s-title">优秀成果推介</div>
      </div>
      <div>
        <el-tabs type="border-card" v-model="activeName">
          <el-tab-pane label="优秀成果引入推介" name="User">
            <!-- 树形结构开始 -->
            <el-tree style="max-width: 600px" :data="data" :props="defaultProps" />
            <!-- 树形结构结束 -->
          </el-tab-pane>
          <el-tab-pane label="自研成果推介" name="Config">
            <ul>
              <li>
                <span>1</span>
                <span class="st-ml">xxxxxxx问题</span>
              </li>
              <li>
                <span>1</span>
                <span class="st-ml">xxxxxxx问题</span>
              </li>
              <li>
                <span>1</span>
                <span class="st-ml">xxxxxxx问题</span>
              </li>
            </ul>
          </el-tab-pane>
          <el-tab-pane label="区县最佳成果实践案例" name="contry_city">
            区县最佳成果实践案例
          </el-tab-pane>
        </el-tabs>
      </div>
    </div>
    <!-- 开始 -->
    <!-- 结束 -->
  </div>
</template>


<script setup lang="ts">

defineOptions({
  name: "Excellentachievements",
  inheritAttrs: false,
})
interface Tree {
  label: string
  children?: Tree[]
}
const data: Tree[] = [
  {
    label: 'Level one 1',
    children: [
      {
        label: 'Level two 1-1',
        children: [
          {
            label: 'Level three 1-1-1',
          },
        ],
      },
    ],
  },
  {
    label: 'Level one 2',
    children: [
      {
        label: 'Level two 2-1',
        children: [
          {
            label: 'Level three 2-1-1',
          },
        ],
      },
      {
        label: 'Level two 2-2',
        children: [
          {
            label: 'Level three 2-2-1',
          },
        ],
      },
    ],
  },
  {
    label: 'Level one 3',
    children: [
      {
        label: 'Level two 3-1',
        children: [
          {
            label: 'Level three 3-1-1',
          },
        ],
      },
      {
        label: 'Level two 3-2',
        children: [
          {
            label: 'Level three 3-2-1',
          },
        ],
      },
    ],
  },
]

const defaultProps = {
  children: 'children',
  label: 'label',
}
const activeName = ref('User');
onMounted(() => {



});

</script>

<style lang="scss" scoped>
.Excellentachievements {
  margin-top: 20px;
  // width: 400px;
  height: 338px;

  .ExpertTeammanagement-bc {
    background-color: #fff;
    height: 338px;

    .h3_pad {
      padding: 10px;
    }

    .st-ml {
      margin-left: 10px;
    }
  }

  .s-title {
    color: #6d6c6c;
    font-weight: bold;
    // padding: 12px 15px;
    // margin-bottom: 20px;
  }
}
</style>