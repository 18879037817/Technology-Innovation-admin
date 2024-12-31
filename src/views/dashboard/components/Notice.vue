<template>
  <div class="Notice">
    <el-row :gutter="10">

      <el-col :xs="24" :span="24">
        <el-card>
          <template #header>
            <div class="flex-x-between">
              <div class="flex-y-center">通知公告</div>
              <el-link type="primary">
                <span class="text-xs" @click="viewMoreNotice">查看更多</span>
                <el-icon class="text-xs">
                  <ArrowRight />
                </el-icon>
              </el-link>
            </div>
          </template>

          <el-scrollbar height="340">
            <div v-for="(item, index) in notices" :key="index" class="flex-y-center py-3">
              <DictLabel v-model="item.type" code="notice_type" size="small" />
              <el-text truncated class="!mx-2 flex-1 !text-xs !text-[var(--el-text-color-secondary)] st-size">
                {{ item.title }}
              </el-text>
              <el-link @click="viewNoticeDetail(item.id)">
                <el-icon class="text-sm">
                  <View />
                </el-icon>
              </el-link>
            </div>
          </el-scrollbar>
        </el-card>
      </el-col>
    </el-row>

    <NoticeDetail ref="noticeDetailRef" />
  </div>
</template>

<script setup lang="ts">
import NoticeAPI, { NoticePageVO } from "@/api/system/notice";
import router from "@/router";
defineOptions({
  name: "Notice",
  inheritAttrs: false,
});
const notices = ref<NoticePageVO[]>([]);
const noticeDetailRef = ref();
// 阅读通知公告
const viewNoticeDetail = (id: string) => {
  noticeDetailRef.value.openNotice(id);
}
onMounted(() => {

  // 获取我的通知公告
  NoticeAPI.getMyNoticePage({ pageNum: 1, pageSize: 10 }).then((data) => {
    notices.value = data.list;
  });
  console.log('aaaaa')

});
// 查看更多
function viewMoreNotice() {
  router.push({ path: "/myNotice" });
}
</script>

<style lang="scss" scoped>
.Notice {

  // box-shadow: 3px 3px 10px #e0e0e0;
  // border-radius: 5px;
  // background-color: #ffffff;
  // padding: 20px;
  .st-size {
    font-size: 14px !important;
  }
}
</style>
