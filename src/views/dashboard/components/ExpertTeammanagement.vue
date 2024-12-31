<template>
  <div class="ExpertTeammanagement">
    <div class="ExpertTeammanagement-bc">
      <div class="h3_pad">
        <div class="s-title">专家团队管理模块</div>
      </div>
      <div>
        <el-tabs type="border-card" v-model="activeName">
          <el-tab-pane label="团队项目展示" name="User">
            <!-- 树形结构开始 -->
            <div>团队项目展示</div>
            <!-- 树形结构结束 -->
          </el-tab-pane>
          <el-tab-pane label="一线问题解决" name="Config">
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
        </el-tabs>
      </div>
    </div>
    <!-- 开始 -->
    <!-- 结束 -->
  </div>
</template>


<script setup lang="ts">

defineOptions({
  name: "ExpertTeammanagement",
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
.ExpertTeammanagement {
  margin-top: 20px;
  margin-left: 20px;
  height: 338px;
  // width: 400px;

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

  .el-tabs--border-card {
    border: none;
  }

  .s-title {
    color: #6d6c6c;
    font-weight: bold;
    // padding: 12px 15px;
    // margin-bottom: 20px;
  }
}
</style>