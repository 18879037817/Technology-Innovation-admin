<template>
  <div class="app-container">
    <div class="flex flex-bet">

      <div class="flex-l PointsManagement-box">
        <el-tabs v-model="activeName" class="demo-tabs">
          <el-tab-pane label="User" name="first">User</el-tab-pane>
          <el-tab-pane label="Config" name="second">Config</el-tab-pane>
          <el-tab-pane label="Role" name="third">Role</el-tab-pane>
          <el-tab-pane label="Task" name="fourth">Task</el-tab-pane>
        </el-tabs>
        <el-input v-model="filterText" style="width: 100%;" placeholder="部门名称" />
        <el-tree ref="treeRef" style="width:100%; margin-top:15px; color:#606266;" class="filter-tree" :data="data"
          :props="defaultProps" default-expand-all :filter-node-method="filterNode" />
      </div>
      <div class="flex-r">
        <div class="search-bar">
          <el-form ref="queryFormRef" :model="queryParams" :inline="true">
            <el-form-item label="通知标题" prop="title">
              <el-input v-model="queryParams.title" placeholder="关键字" clearable @keyup.enter="handleQuery()" />
            </el-form-item>
            <el-form-item>
              <el-button type="primary" @click="handleQuery()">
                <template #icon>
                  <Search />
                </template>
                搜索
              </el-button>
              <el-button @click="handleResetQuery()">
                <template #icon>
                  <Refresh />
                </template>
                重置
              </el-button>
            </el-form-item>
          </el-form>
        </div>

        <el-card shadow="never">
          <el-table ref="dataTableRef" v-loading="loading" :data="pageData" highlight-current-row>
            <el-table-column type="index" label="序号" width="60" />
            <el-table-column label="内容" prop="title" min-width="200" />
            <el-table-column align="center" label="类型" width="150">
              <template #default="scope">
                <el-tag type="primary">{{ scope.row.pointsType == 0 ? '积分' : '需求上报' }}</el-tag>
              </template>
            </el-table-column>
            <el-table-column align="center" label="发布人" prop="publisherName" width="100" />
            <el-table-column align="center" label="通知等级" width="100">
              <template #default="scope">
                <DictLabel v-model="scope.row.level" code="notice_level" />
              </template>
            </el-table-column>
            <el-table-column key="releaseTime" align="center" label="发布时间" prop="publishTime" width="150" />

            <el-table-column align="center" label="发布人" prop="publisherName" width="150" />
            <el-table-column align="center" label="状态" width="100">
              <template #default="scope">
                <el-tag v-if="scope.row.isRead == 1" type="success">已读</el-tag>
                <el-tag v-else type="info">未读</el-tag>
              </template>
            </el-table-column>
            <el-table-column align="center" fixed="right" label="操作" width="80">
              <template #default="scope">
                <el-button type="primary" size="small" link @click="viewNoticeDetail(scope.row.id)">
                  查看
                </el-button>
              </template>
            </el-table-column>
          </el-table>

          <pagination v-if="total > 0" v-model:total="total" v-model:page="queryParams.pageNum"
            v-model:limit="queryParams.pageSize" @pagination="handleQuery()" />
        </el-card>

        <NoticeDetail ref="noticeDetailRef" />
      </div>
    </div>


  </div>
</template>

<script setup lang="ts">

import { ElTree } from 'element-plus'
defineOptions({
  name: "PointsManagement",
  inheritAttrs: false,
});
interface Tree {
  [key: string]: any
}

const filterText = ref('')
const treeRef = ref<InstanceType<typeof ElTree>>()
const activeName = ref('first');
const defaultProps = {
  children: 'children',
  label: 'label',
}
watch(filterText, (val) => {
  treeRef.value!.filter(val)
})
const filterNode = (value: string, data: Tree) => {
  if (!value) return true
  return data.label.includes(value)
}
const data: Tree[] = [
  {
    id: 1,
    label: 'Level one 1',
    children: [
      {
        id: 4,
        label: 'Level two 1-1',
        children: [
          {
            id: 9,
            label: 'Level three 1-1-1',
          },
          {
            id: 10,
            label: 'Level three 1-1-2',
          },
        ],
      },
    ],
  },
  {
    id: 2,
    label: 'Level one 2',
    children: [
      {
        id: 5,
        label: 'Level two 2-1',
      },
      {
        id: 6,
        label: 'Level two 2-2',
      },
    ],
  },
  {
    id: 3,
    label: 'Level one 3',
    children: [
      {
        id: 7,
        label: 'Level two 3-1',
      },
      {
        id: 8,
        label: 'Level two 3-2',
      },
    ],
  },
]
import NoticeAPI, { NoticePageVO, NoticePageQuery } from "@/api/system/notice";

const queryFormRef = ref(ElForm);
const noticeDetailRef = ref();
const points = ref([
  {
    id: 0,
    name: '积分'
  },
  {
    id: 1,
    name: '需求上报'
  },
])
const pageData = ref<NoticePageVO[]>([]);

const loading = ref(false);
const total = ref(0);

const queryParams = reactive<NoticePageQuery>({
  pageNum: 1,
  pageSize: 10,
});

/** 查询通知公告 */
function handleQuery() {
  loading.value = true;
  NoticeAPI.getMyNoticePage(queryParams)
    .then((data) => {
      data.list.forEach((v: any) => {
        v.pointsType = 0;
      })
      pageData.value = data.list;

      total.value = data.total;
    })
    .finally(() => {
      loading.value = false;
    });
}

/** 重置通知公告查询 */
function handleResetQuery() {
  queryFormRef.value!.resetFields();
  queryParams.pageNum = 1;
  handleQuery();
}

function viewNoticeDetail(id: string) {
  noticeDetailRef.value.openNotice(id);
}

onMounted(() => {
  handleQuery();
});
</script>
<style scoped>
.PointsManagement-box {
  padding: 20px;
}

.flex-bet {
  justify-content: space-between;
}

.flex-l {
  width: 16%;
  background-color: #fff;
}

.flex-r {
  width: 82.8%;

}
</style>
