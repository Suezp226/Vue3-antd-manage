<template>
  <a-tabs v-model:activeKey="activeKey" hide-add type="editable-card" @edit="onEdit" style="height:100%;" 
    :tabBarStyle="{'margin': 0}">
    <a-tab-pane v-for="pane in panes" :key="pane.key" :tab="pane.title" :closable="pane.closable" 
      :style="{ margin: '0px', padding: '0 20px', background: '#fff', minHeight: '280px' }">
      <div class="fix-top" ></div>
      <slot></slot>
      <div class="fix-right" ></div>
    </a-tab-pane>
  </a-tabs>
</template>
<script lang="ts">
import { defineComponent, ref } from 'vue';
import { useRouter, useRoute } from 'vue-router';

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

    const add = (title: string, content: string, key: string) => {
      activeKey.value = `newTab${newTabIndex.value++}`;
      panes.value.push({
        title: title,
        content: content,
        key: key
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


    const router = useRouter();
    const onEdit = (targetKey: string) => {
      remove(targetKey);
      if(panes.value.length == 0) {
        add('Home','Home-content','home');
        activeKey.value = 'home';
        router.push({
          path: '/home'
        })
      }
      // 这里应该添加一个Home
      
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
    position: relative;
    box-sizing: border-box;
    overflow-y: auto;
    overflow-x: hidden;
  }
  .ant-tabs-nav {
    padding: 8px 0 0 14px;
    height: 42px;
  }
  .fix-top {
    position: relative;
    width: 140%;
    left: -20px;
    height: 10px;
    background-color:#f0f2f5;
  }
  .fix-right {
    position: absolute;
    height: 100%;
    right: 0;
    top: 0px;
    width: 15px;
    background: #f0f2f5;
  }
</style>

