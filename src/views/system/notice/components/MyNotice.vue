<template>
  <div class="app-container">
    <div class="search-bar">

      <!-- 开始 -->
      <el-tabs v-model="activeName" class="demo-tabs">
        <el-tab-pane :label="item.name" :name="item.id" v-for="(item, index) in TabData">
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

              <el-button type="success">批量导出</el-button>
            </el-form-item>
          </el-form>
        </el-tab-pane>

      </el-tabs>
      <!-- 结束 -->

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
        <el-table-column align="center" fixed="right" label="操作">
          <template #default="scope">
            <el-button type="primary" size="small" link @click="viewNoticeDetail(scope.row.id)">
              查看
            </el-button>
            <el-button type="primary" size="small" link @click="handleDelete(scope.row.id)">通过</el-button>

            <el-button type="primary" size="small" link @click="reject">驳回</el-button>
          </template>
        </el-table-column>
      </el-table>

      <pagination v-if="total > 0" v-model:total="total" v-model:page="queryParams.pageNum"
        v-model:limit="queryParams.pageSize" @pagination="handleQuery()" />
    </el-card>

    <NoticeDetail ref="noticeDetailRef" />

    <!-- 驳回弹窗开始 -->
    <el-dialog v-model="dialog.visible" :title="dialog.title" width="600px" @close="handleCloseDialog">
      <el-form ref="roleFormRef" :model="formData" :rules="rules" label-width="100px">
        <el-form-item label="驳回原因" prop="release">
          <el-input v-model="formData.release" placeholder="请输入驳回原因" />
        </el-form-item>
      </el-form>
      <template #footer>
        <div class="dialog-footer">
          <el-button type="primary" @click="handleSubmit">确 定</el-button>
          <el-button @click="handleCloseDialog">取 消</el-button>
        </div>
      </template>
    </el-dialog>
    <!-- 驳回弹窗结束 -->

  </div>
</template>

<script setup lang="ts">
defineOptions({
  name: "MyNotice",
  inheritAttrs: false,
});

import NoticeAPI, { NoticePageVO, NoticePageQuery } from "@/api/system/notice";

const queryFormRef = ref(ElForm);
const noticeDetailRef = ref();
const rules = reactive({
  release: [{ required: true, message: "请输入驳回原因", trigger: "blur" }],
});
const activeName = ref('ProjectReview');
const TabData = reactive([
  {
    id: 'ProjectReview',
    name: '项目审核'
  },
  {
    id: 'Personalpointsreview',
    name: '个人积分审核'
  },
  {
    id: 'Unitpointreview',
    name: '单位积分审核'
  }
])
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

const dialog = reactive({
  title: "驳回",
  visible: false,
});

const formData = reactive({
  release: ''
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
const handleSubmit = () => {
  dialog.visible = false;
}

const handleCloseDialog = () => {
  dialog.visible = false;
}
const reject = () => {    //驳回
  dialog.visible = true;
}

// 删除系统配置
function handleDelete(id: number) {
  ElMessageBox.confirm("确认通过吗?", "提示", {
    confirmButtonText: "确定",
    cancelButtonText: "取消",
    type: "info",
  }).then(() => {
    loading.value = false;
  });
}
onMounted(() => {
  handleQuery();
});
</script>
