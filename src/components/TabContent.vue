<template>
  <a-tabs v-model:activeKey="activeKey" hide-add type="editable-card" @edit="onEdit" style="height:100%;" 
    :tabBarStyle="{'margin': 0}">
    <a-tab-pane v-for="pane in panes" :key="pane.key" :tab="pane.title" :closable="pane.closable" 
      :style="{ margin: '0px', padding: '0 20px', background: '#fff', minHeight: '280px' }">
      <div class="fix-top" ></div>
      <slot></slot>
    </a-tab-pane>
  </a-tabs>
</template>
<script lang="ts">
import { defineComponent, ref } from 'vue';

export default defineComponent({
  setup() {
    const panes = ref<{ title: string; content: string; key: string; closable?: boolean }[]>(
      new Array(2).fill(null).map((_, index) => {
        const id = String(index + 1);
        return { title: `Tab ${id}`, content: `Content of Tab Pane ${id}`, key: id };
      }),
    );
    const activeKey = ref(panes.value[0].key);
    const newTabIndex = ref(0);

    const add = () => {
      activeKey.value = `newTab${newTabIndex.value++}`;
      panes.value.push({
        title: `New Tab ${activeKey.value}`,
        content: `Content of new Tab ${activeKey.value}`,
        key: activeKey.value,
      });
    };

    const remove = (targetKey: string) => {
      let lastIndex = 0;
      panes.value.forEach((pane, i) => {
        if (pane.key === targetKey) {
          lastIndex = i - 1;
        }
      });
      panes.value = panes.value.filter(pane => pane.key !== targetKey);
      if (panes.value.length && activeKey.value === targetKey) {
        if (lastIndex >= 0) {
          activeKey.value = panes.value[lastIndex].key;
        } else {
          activeKey.value = panes.value[0].key;
        }
      }
    };

    const onEdit = (targetKey: string) => {
      remove(targetKey);
    };

    return {
      panes,
      activeKey,
      onEdit,
      add,
    };
  },
});
</script>

<style lang="less">
  .ant-tabs-content-holder {
    height: 100%;
    background: #fff;
    margin: 0px 0px 20px 15px;
    box-sizing: border-box;
    overflow-y: auto;
    overflow-x: hidden;
  }
  .fix-top {
    position: relative;
    width: 140%;
    left: -20px;
    height:20px;
    background-color:#f0f2f5;
  }
</style>

