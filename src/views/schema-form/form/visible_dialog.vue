<script setup lang="ts">
import { ref } from "vue";
// https://plus-pro-components.com/components/dialog-form.html
import "plus-pro-components/es/components/dialog-form/style/css";
import {
  type PlusColumn,
  type FieldValues,
  PlusDialogForm
} from "plus-pro-components";

const columns: PlusColumn[] = [
  {
    label: "New fingerprint",
    width: 120,
    prop: "name",
    valueType: "copy"
  },
  {
    label: "Profile name",
    width: 120,
    prop: "name",
    valueType: "copy"
  },
  {
    label: "Browser",
    width: 100,
    prop: "name",
    valueType: "copy"
  },
  {
    label: "Browser version",
    prop: "time",
    valueType: "date-picker"
  },
  {
    label: "OS",
    prop: "name",
    valueType: "copy"
  },
  {
    label: "Proxy",
    prop: "city",
    valueType: "cascader",
    options: [
      {
        value: "0",
        label: "陕西",
        children: [
          {
            value: "0-0",
            label: "西安",
            children: [
              {
                value: "0-0-0",
                label: "新城区"
              },
              {
                value: "0-0-1",
                label: "高新区"
              },
              {
                value: "0-0-2",
                label: "灞桥区"
              }
            ]
          }
        ]
      },
      {
        value: "1",
        label: "山西",
        children: [
          {
            value: "1-0",
            label: "太原",
            children: [
              {
                value: "1-0-0",
                label: "小店区"
              },
              {
                value: "1-0-1",
                label: "古交市"
              },
              {
                value: "1-0-2",
                label: "万柏林区"
              }
            ]
          }
        ]
      }
    ]
  },
  {
    label: "Type",
    prop: "place",
    tooltip: "请精确到门牌号",
    fieldProps: {
      placeholder: "请精确到门牌号"
    }
  },
  {
    label: "Proxy Host",
    prop: "demand",
    valueType: "copy"
  }
];

const visible = ref(true);
const values = ref<FieldValues>({});
</script>

<template>
  <div>
    <PlusDialogForm
      v-model:visible="visible"
      v-model="values"
      :form="{ columns }"
      title="Details"
    />
  </div>
</template>
