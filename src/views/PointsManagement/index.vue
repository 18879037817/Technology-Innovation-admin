<template>
  <div class="app-container">
    <div class="flex flex-bet">

      <div class="flex-l PointsManagement-box">
        <el-tabs v-model="activeName" class="demo-tabs">
          <el-tab-pane label="个人积分" name="Personalpoints">
            <el-input v-model="filterText" style="width: 100%;" placeholder="部门名称" />
            <el-tree ref="treeRef" style="width:100%; margin-top:15px; color:#606266;" class="filter-tree"
              :data="Personalpointsdata" :props="defaultProps" default-expand-all :filter-node-method="filterNode" />
          </el-tab-pane>
          <el-tab-pane label="单位积分" name="UnitPoints">
            <el-input v-model="filterText" style="width: 100%;" placeholder="部门名称" />
            <el-tree ref="treeRef" style="width:100%; margin-top:15px; color:#606266;" class="filter-tree"
              :data="UnitPointsdata" :props="defaultProps" default-expand-all :filter-node-method="filterNode" />
          </el-tab-pane>
        </el-tabs>

      </div>
      <div class="flex-r">
        <div class="search-bar">
          <el-form ref="queryFormRef" :model="queryParams" :inline="true">
            <el-form-item label="申报人" prop="title">
              <el-input v-model="queryParams.title" placeholder="请输入申报人" clearable @keyup.enter="handleQuery()" />
            </el-form-item>
            <el-form-item label="状态" prop="status">
              <el-select v-model="queryParams.status" placeholder="全部" clearable class="!w-[100px]">
                <el-option label="正常" :value="1" />
                <el-option label="禁用" :value="0" />
              </el-select>
            </el-form-item>

            <el-form-item label="创建时间">
              <el-date-picker v-model="queryParams.createTime" :editable="false" class="!w-[240px]" type="daterange"
                range-separator="~" start-placeholder="开始时间" end-placeholder="截止时间" value-format="YYYY-MM-DD" />
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

            <div class="mb-10px">
              <el-button type="success" icon="plus" @click="handleOpenDialog()">新增</el-button>
              <el-button type="danger">
                删除
              </el-button>
            </div>
          </el-form>
        </div>

        <el-card shadow="never">
          <el-table ref="dataTableRef" v-loading="loading" :data="pageData" highlight-current-row>
            <el-table-column type="index" label="编号" width="60" />
            <el-table-column align="center" label="申报人" prop="publisherName" />
            <el-table-column align="center" label="积分详情">
              <template #default="scope">
                <div>{{ scope.row.pointsType == 0 ? '积分' : '需求上报' }}</div>
              </template>
            </el-table-column>
            <el-table-column align="center" label="状态">
              <template #default="scope">
                <el-tag v-if="scope.row.isRead == 1" type="success">已读</el-tag>
                <el-tag v-else type="info">未读</el-tag>
              </template>
            </el-table-column>
            <el-table-column key="releaseTime" align="center" label="创建时间" prop="publishTime" />
            <el-table-column align="center" fixed="right" label="操作">
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

    <!-- 开始 -->
    <!-- 角色表单弹窗 -->
    <el-dialog v-model="dialog.visible" :title="dialog.title" width="600px" @close="handleCloseDialog">
      <el-form ref="roleFormRef" :model="formData" :rules="rules" label-width="100px">
        <el-form-item label="项目名称" prop="name">
          <el-input v-model="formData.name" placeholder="请输入项目名称" />
        </el-form-item>
        <el-form-item label="加分类项目" prop="dataScope">
          <el-select v-model="formData.dataScope">
            <el-option :key="0" label="全部数据" :value="0" />
            <el-option :key="1" label="部门及子部门数据" :value="1" />
            <el-option :key="2" label="本部门数据" :value="2" />
            <el-option :key="3" label="本人数据" :value="3" />
          </el-select>
        </el-form-item>
        <el-form-item label="创建时间">
          <el-date-picker v-model="formData.createTime" :editable="false" class="!w-[240px]" type="daterange"
            range-separator="~" start-placeholder="开始时间" end-placeholder="截止时间" value-format="YYYY-MM-DD" />
        </el-form-item>

        <!-- 开始 -->
        <el-form-item label="活动素材">
          <el-upload action="#" list-type="picture-card" :auto-upload="false">
            <el-icon>
              <Plus />
            </el-icon>

            <template #file="{ file }">
              <div>
                <img class="el-upload-list__item-thumbnail" :src="file.url" alt="" />
                <span class="el-upload-list__item-actions">
                  <span class="el-upload-list__item-preview" @click="handlePictureCardPreview(file)">
                    <el-icon><zoom-in /></el-icon>
                  </span>
                  <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleDownload(file)">
                    <el-icon>
                      <Download />
                    </el-icon>
                  </span>
                  <span v-if="!disabled" class="el-upload-list__item-delete" @click="handleRemove(file)">
                    <el-icon>
                      <Delete />
                    </el-icon>
                  </span>
                </span>
              </div>
            </template>
          </el-upload>

          <el-dialog v-model="dialogVisible">
            <img w-full :src="dialogImageUrl" alt="Preview Image" />
          </el-dialog>
        </el-form-item>

        <!-- 结束 -->
      </el-form>

      <template #footer>
        <div class="dialog-footer">
          <el-button type="primary" @click="handleSubmit">确 定</el-button>
          <el-button @click="handleCloseDialog">取 消</el-button>
        </div>
      </template>
    </el-dialog>
    <!-- 结束 -->
  </div>
</template>

<script setup lang="ts">
import type { ElTree, UploadFile } from 'element-plus'
import { Delete, Download, Plus, ZoomIn } from '@element-plus/icons-vue'
defineOptions({
  name: "PointsManagement",
  inheritAttrs: false,
});
interface Tree {
  [key: string]: any
}
const rules = reactive({
  name: [{ required: true, message: "请输入角色名称", trigger: "blur" }],
  code: [{ required: true, message: "请输入角色编码", trigger: "blur" }],
  dataScope: [{ required: true, message: "请选择数据权限", trigger: "blur" }],
  status: [{ required: true, message: "请选择状态", trigger: "blur" }],
});
const filterText = ref('')
const treeRef = ref<InstanceType<typeof ElTree>>()
const activeName = ref('Personalpoints');
const roleFormRef = ref(ElForm);
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
// 弹窗
const dialog = reactive({
  title: "新增",
  visible: false,
});
const UnitPointsdata: Tree[] = [
  {
    id: 1,
    label: '科创成效',
    children: [
      {
        id: 4,
        label: '科创项目增收创利',
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
      {
        id: 4,
        label: '科创项目降本增效',
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
      }
    ],
  },
  {
    id: 2,
    label: '评优评先',
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
    label: '科创氛围',
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

const Personalpointsdata: Tree[] = [
  {
    id: 1,
    label: '项目类',
    children: [
      {
        id: 4,
        label: '国家级别',
        children: [
          {
            id: 9,
            label: '一类',
          },
          {
            id: 10,
            label: '二类',
          },
          {
            id: 10,
            label: '三类',
          }
        ],
      },
      {
        id: 4,
        label: '省部级',
        children: [
          {
            id: 9,
            label: '一类',
          },
          {
            id: 10,
            label: '二类',
          },
          {
            id: 10,
            label: '三类',
          }
        ],
      },
    ],
  },
  {
    id: 2,
    label: '成果奖励类',
    children: [
      {
        id: 4,
        label: '国家级别',
        children: [
          {
            id: 9,
            label: '专项类',
          },
          {
            id: 10,
            label: '专业类',
          }
        ],
      },
      {
        id: 4,
        label: '省部级',
        children: [
          {
            id: 9,
            label: '专项类',
          },
          {
            id: 10,
            label: '专业类',
          }
        ]
      },
    ],
  },
  {
    id: 3,
    label: '产业创新类',
    children: [
      {
        id: 7,
        label: '高水平创新平台',
      },
      {
        id: 8,
        label: '新增自主可控实践创新突破',
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


const dialogImageUrl = ref('')
const dialogVisible = ref(false)
const disabled = ref(false)

const handleRemove = (file: UploadFile) => {
  console.log(file)
}

const handlePictureCardPreview = (file: UploadFile) => {
  dialogImageUrl.value = file.url!
  dialogVisible.value = true
}

const handleDownload = (file: UploadFile) => {
  console.log(file)
}
const pageData = ref<NoticePageVO[]>([]);

const loading = ref(false);
const total = ref(0);

const queryParams = reactive<NoticePageQuery>({
  pageNum: 1,
  pageSize: 10,
  status: 1
});

// 角色表单
const formData = reactive({
  id: undefined,
  sort: 1,
  status: 2,
  name: '',
  code: '',
  dataScope: '',
  createTime: []
});
// 关闭弹窗
function handleCloseDialog() {
  dialog.visible = false;

  roleFormRef.value.resetFields();
  roleFormRef.value.clearValidate();

  formData.id = undefined;
  formData.sort = 1;
  formData.status = 1;
}
// 新增
const handleOpenDialog = () => {
  dialog.visible = true;
}
// 提交角色表单
function handleSubmit() {
  roleFormRef.value.validate((valid: any) => {
    if (valid) {

    }
  });
}
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
