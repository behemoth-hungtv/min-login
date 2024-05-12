<script setup lang="ts">
import { useRole } from "./utils/hook";
import { ref, computed, nextTick, onMounted } from "vue";
import { PureTableBar } from "@/components/RePureTableBar";
import { useRenderIcon } from "@/components/ReIcon/src/hooks";
import {
  delay,
  subBefore,
  deviceDetection,
  useResizeObserver
} from "@pureadmin/utils";

// import Database from "@iconify-icons/ri/database-2-line";
// import More from "@iconify-icons/ep/more-filled";
import Delete from "@iconify-icons/ep/delete";
import EditPen from "@iconify-icons/ep/edit-pen";
import Refresh from "@iconify-icons/ep/refresh";
import Menu from "@iconify-icons/ep/menu";
import AddFill from "@iconify-icons/ri/add-circle-line";
import Close from "@iconify-icons/ep/close";
import Check from "@iconify-icons/ep/check";
import GithubNavigationTab from "@/components/ReCustom/GithubNavigationTab.vue";
import More from "@iconify-icons/ep/more-filled";
import TestVue from "@/views/system/role/test.vue";

defineOptions({
  name: "SystemRole"
});

const iconClass = computed(() => {
  return [
    "w-[22px]",
    "h-[22px]",
    "flex",
    "justify-center",
    "items-center",
    "outline-none",
    "rounded-[4px]",
    "cursor-pointer",
    "transition-colors",
    "hover:bg-[#0000000f]",
    "dark:hover:bg-[#ffffff1f]",
    "dark:hover:text-[#ffffffd9]"
  ];
});

const treeRef = ref();
const formRef = ref();
const tableRef = ref();
const contentRef = ref();
const treeHeight = ref();

const rendContent = (val: string) =>
  `代码位置：src/views/table/edit/${val}/index.vue`;

const functionButtonsData: any = [
  {
    id: 1,
    text: "Start 1 profiles",
    action: "startProfiles",
    type: "primary",
    iconClass: "play-icon"
  },
  {
    id: 2,
    text: "Stop 1 profiles",
    action: "stopProfiles",
    iconClass: "stop-icon",
    type: "danger",
    plain: true
  },
  {
    id: 3,
    text: "Assign to group",
    action: "assignGroup",
    iconClass: "user-plus-icon",
    type: "primary"
  },
  {
    id: 4,
    text: "Share profiles",
    action: "shareProfiles",
    iconClass: "share-icon",
    type: "primary"
  },
  {
    id: 5,
    text: "Check proxy",
    action: "checkProxy",
    iconClass: "check-icon",
    type: "primary"
  },
  {
    id: 6,
    text: "Start with app",
    action: "startWithApp",
    iconClass: "start-app-icon",
    type: "primary",
    plain: true
  },
  {
    id: 7,
    text: "Update proxy",
    action: "updateProxy",
    iconClass: "update-icon",
    type: "primary",
    plain: true
  },
  {
    id: 8,
    text: "Update profiles",
    action: "updateProfiles",
    iconClass: "update-profiles-icon",
    type: "primary",
    plain: true
  },
  {
    id: 9,
    text: "Share on cloud",
    action: "shareOnCloud",
    iconClass: "cloud-icon",
    type: "danger",
    plain: true
  },
  {
    id: 9,
    text: "New Finger Print",
    action: "new finger print",
    iconClass: "icon-class",
    type: "success",
    plain: true
  }
];

const list = [
  {
    key: "demo1",
    content: rendContent("Cloud"),
    title: "整体编辑",
    component: TestVue
  },
  {
    key: "demo1",
    content: rendContent("Local"),
    title: "整体编辑",
    component: TestVue
  },
  {
    key: "demo1",
    content: rendContent("Group"),
    title: "整体编辑",
    component: TestVue
  },
  {
    key: "demo1",
    content: rendContent("Team"),
    title: "整体编辑",
    component: TestVue
  }
];

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

const {
  form,
  isShow,
  curRow,
  loading,
  columns,
  rowStyle,
  dataList,
  treeData,
  treeProps,
  isLinkage,
  pagination,
  isExpandAll,
  isSelectAll,
  treeSearchValue,
  buttonClass,
  // buttonClass,
  onSearch,
  resetForm,
  openDialog,
  handleMenu,
  handleSave,
  handleDelete,
  filterMethod,
  transformI18n,
  onQueryChanged,
  // handleDatabase,
  handleSizeChange,
  handleCurrentChange,
  handleSelectionChange
} = useRole(treeRef);
</script>

<template>
  <div class="main">
    <div>
      <section class="search-section">
        <el-form
          ref="formRef"
          :inline="true"
          :model="form"
          class="search-form bg-bg_color overflow-auto"
        >
          <el-form-item prop="name">
            <el-input
              v-model="form.name"
              placeholder="请输入菜单名称"
              clearable
              class="!w-[180px]"
            />
          </el-form-item>
          <el-form-item>
            <el-button
              type="primary"
              :icon="useRenderIcon('ri:search-line')"
              :loading="loading"
              @click="onSearch"
            >
              搜索
            </el-button>
          </el-form-item>
        </el-form>
      </section>

      <section class="functional-section flex flex-wrap gap-1.5 mt-3">
        <el-button
          v-for="data in functionButtonsData"
          :type="data.type"
          :plain="data.plain"
          :icon="useRenderIcon(EditPen)"
          @click="resetForm(formRef)"
          class="mx-0"
        >
          {{ data.text }}
        </el-button>

        <el-dropdown>
          <el-button
            class="ml-3 mt-[2px]"
            link
            type="primary"
            :icon="useRenderIcon(More)"
          />
          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item>
                <el-button
                  :class="buttonClass"
                  link
                  type="default"
                  :icon="useRenderIcon(Menu)"
                  @click="handleMenu"
                >
                  菜单权限
                </el-button>
              </el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </section>
    </div>

    <div ref="contentRef">
      <PureTableBar
        style="transition: width 220ms cubic-bezier(0.4, 0, 0.2, 1)"
        title=""
        :columns="columns"
        @refresh="onSearch"
      >
        <template v-slot="{ size, dynamicColumns }">
          <pure-table
            ref="tableRef"
            row-key="id"
            :adaptiveConfig="{ offsetBottom: 108 }"
            align-whole="center"
            table-layout="auto"
            stripe
            :loading="loading"
            :size="size"
            :data="dataList"
            :columns="dynamicColumns"
            :pagination="pagination"
            :paginationSmall="size === 'small' ? true : false"
            :header-cell-style="{
              background: 'var(--el-fill-color-light)',
              color: 'var(--el-text-color-primary)'
            }"
            @selection-change="handleSelectionChange"
            @page-size-change="handleSizeChange"
            @page-current-change="handleCurrentChange"
          >
            <template #operation="{ row }">
              <el-button
                class="reset-margin"
                type="primary"
                @click="openDialog('修改', row)"
              >
                Start
              </el-button>

              <el-dropdown>
                <el-button
                  class="ml-3 mt-[2px]"
                  link
                  type="primary"
                  :size="size"
                  :icon="useRenderIcon(More)"
                />
                <template #dropdown>
                  <el-dropdown-menu>
                    <el-dropdown-item>
                      <el-button
                        :class="buttonClass"
                        link
                        type="primary"
                        :size="size"
                        :icon="useRenderIcon(Menu)"
                        @click="handleMenu"
                      >
                        菜单权限
                      </el-button>
                    </el-dropdown-item>
                  </el-dropdown-menu>
                </template>
              </el-dropdown>
            </template>
          </pure-table>
        </template>
      </PureTableBar>
    </div>
  </div>
</template>

<style scoped lang="scss">
:deep(.el-dropdown-menu__item i) {
  margin: 0;
}

:deep(.functional-section button) {
  margin-left: 0 !important;
}

.main-content {
  margin: 24px 24px 0 !important;
}

.search-form {
  :deep(.el-form-item) {
    margin-bottom: 12px;
  }
}
</style>
