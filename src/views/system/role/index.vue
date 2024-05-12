<script setup lang="ts">
import { ref, computed, nextTick, onMounted } from "vue";
import { delay, subBefore, useResizeObserver } from "@pureadmin/utils";

import DialogVue from "@/views/schema-form/form/visible_dialog.vue";
import TestVue from "@/views/system/role/test.vue";

defineOptions({
  name: "SystemRole"
});

const tableRef = ref();
const contentRef = ref();
const treeHeight = ref();

const rendContent = (val: string) =>
  `代码位置：src/views/table/edit/${val}/index.vue`;

const list = [
  {
    key: "demo1",
    content: rendContent("Cloud"),
    title: "Cloud",
    component: TestVue
  },
  {
    key: "demo1",
    content: rendContent("Local"),
    title: "Local",
    component: TestVue
  },
  {
    key: "demo1",
    content: rendContent("Group"),
    title: "Group",
    component: TestVue
  },
  {
    key: "demo1",
    content: rendContent("Team"),
    title: "Team",
    component: TestVue
  }
];

const selected = ref(0);

function tabClick({ index }) {
  selected.value = index;
}

onMounted(() => {
  useResizeObserver(contentRef, async () => {
    await nextTick();
    delay(60).then(() => {
      treeHeight.value = parseFloat(
        subBefore(tableRef.value.getTableDoms().tableWrapper.style.height, "px")
      );
    });
  });
});
</script>

<template>
  <div class="main">
    <el-row :gutter="16">
      <el-col :xs="24" :sm="24" :md="18" :lg="18" :xl="18">
        <el-card shadow="never">
          <el-tabs @tab-click="tabClick">
            <template v-for="(item, index) of list" :key="item.key">
              <el-tab-pane :lazy="true">
                <template #label>
                  <span
                    v-tippy="{
                      maxWidth: 'none',
                      content: `（第 ${index + 1} 个示例）${item.content}`
                    }"
                  >
                    {{ item.title }}
                  </span>
                </template>
                <component :is="item.component" v-if="selected == index" />
              </el-tab-pane>
            </template>
          </el-tabs>
        </el-card>
      </el-col>

      <el-col :xs="24" :sm="6" :md="6" :lg="6" :xl="6">
        <dialog-vue title="text"></dialog-vue>
      </el-col>
    </el-row>
  </div>
</template>

<style scoped lang="scss">
:deep(.el-dropdown-menu__item i) {
  margin: 0;
}

.main-content {
  margin: 24px 24px 0 !important;
}

.search-form {
  :deep(.el-form-item) {
    margin-bottom: 12px;
  }
}

:deep(.el-overlay) {
  position: relative !important;
  background-color: transparent !important;

  .el-overlay-dialog {
    position: relative !important;
  }

  .el-dialog {
    margin-top: 0 !important;
    width: 100% !important;
  }
}

:deep(.el-form-item__label) {
  width: 150px !important;
}
</style>
