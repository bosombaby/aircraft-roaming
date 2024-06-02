<template>
  <BasePanel title="操作指南">
    <div class="base-tutorial">
      <div class="params-item" v-for="{ value, label } in list">
        <span class="label">{{ label }}:</span>
        <span class="value">{{ value }}</span>
      </div>
    </div>
    <div class="base-info">
      目前为定速巡航模式，请跳转<a
        href="https://aircraft.vrteam.top/"
        target="_blank"
        style="color: skyblue; cursor: pointer"
        >新页面</a
      >进行操作
    </div>
  </BasePanel>
</template>
<script setup lang="ts">
  import BasePanel from "@/components/BasePanel.vue"

  const list = [
    {
      label: "W键",
      value: "俯冲",
    },
    {
      label: "S键",
      value: "升起",
    },
    {
      label: "A键",
      value: "左转",
    },
    {
      label: "D键",
      value: "右转",
    },
    {
      label: "Q键",
      value: "加速",
    },
    {
      label: "E键",
      value: "减速",
    },
  ]
</script>
<style lang="scss" scoped>
  .base-tutorial {
    display: grid;
    grid-gap: 10px;
    grid-template-columns: repeat(2, 1fr);
    padding-top: 4px;

    .params-item {
      display: flex;
      align-items: center;
      font-size: 14px;

      .label {
        margin-right: 4px;
        padding: 2px 10px;
        font-size: 12px;
        background-color: #f0f2f3;
      }

      .value {
        font-size: 12px;
      }
    }
  }

  .base-info {
    margin-top: 14px;
    font-size: 12px;
    color: #222;
  }
</style>
