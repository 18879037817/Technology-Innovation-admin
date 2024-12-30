<template>
  <div class="Notice">
    <el-scrollbar height="400px">
      <div v-for="(item, index) in notices" :key="index" class="flex-y-center py-3">
        <DictLabel v-model="item.type" code="notice_type" size="small" />
        <el-text truncated class="!mx-2 flex-1 !text-xs !text-[var(--el-text-color-secondary)]">
          {{ item.title }}
        </el-text>
        <el-link @click="viewNoticeDetail(item.id)">
          <el-icon class="text-sm">
            <View />
          </el-icon>
        </el-link>
      </div>
    </el-scrollbar>

    <NoticeDetail ref="noticeDetailRef" />
  </div>
</template>

<script setup lang="ts">
import NoticeAPI, { NoticePageVO } from "@/api/system/notice";
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
</script>

<style lang="scss" scoped>
.Notice {
  box-shadow: 3px 3px 10px #e0e0e0;
  border-radius: 5px;
  background-color: #ffffff;
  padding: 20px;
}
</style>
