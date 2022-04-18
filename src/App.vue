<template>
  <a-layout>
    <a-layout-sider v-model:collapsed="collapsed" :trigger="null" collapsible class="shadow">
      <div class="logo" >
        LOGO
      </div>
      <Menu @clickMenu="clickMenu"></Menu>
    </a-layout-sider>


    <a-layout>
      <a-layout-header style="background: #fff; padding: 0;display:flex;">
        <!-- 左侧菜单展开按钮 -->
        <div>
          <menu-unfold-outlined
            v-if="collapsed"
            class="trigger"
            @click="() => (collapsed = !collapsed)"
          />
          <menu-fold-outlined v-else class="trigger" @click="() => (collapsed = !collapsed)" />
        </div>
        
        <!-- Router Tab -->
        <div style="flex:1;overflow:auto;" >
          center
        </div>

        <!-- Info Box -->

        <div class="info-box" >
          <a-avatar :size="{ xs: 24, sm: 32,}">
            <template #icon>
              <user-outlined />
            </template>
          </a-avatar>
          <div class="user-name" >
            Test
          </div>
        </div>

      </a-layout-header>

      <!-- Router 内容 -->
      <a-layout-content
      > 
        <tab-content ref="tabContent">
          <router-view></router-view>
        </tab-content>
      </a-layout-content>
    </a-layout>

  </a-layout>
</template>

<script lang="ts">
import {
  UserOutlined,
  VideoCameraOutlined,
  UploadOutlined,
  MenuUnfoldOutlined,
  MenuFoldOutlined,
} from '@ant-design/icons-vue';
import { defineComponent, ref } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import TabContent from '@/components/TabContent.vue';
import Menu from '@/components/Menu.vue';
export default defineComponent({
  components: {
    UserOutlined,
    VideoCameraOutlined,
    UploadOutlined,
    MenuUnfoldOutlined,
    MenuFoldOutlined,
    TabContent,
    Menu
  },
  setup() {

    const router = useRouter();
    const route = useRoute();
    const tabContent = ref(null);

    let num = 10;

    const clickMenu = function(param: {key: String}) {
      // tabContent.value.add();
      router.push({
        path: '/' + param.key
      })
    }

    return {
      selectedKeys: ref<string[]>(['1']),
      collapsed: ref<boolean>(false),
      num,
      clickMenu,
      tabContent
    };
  },
});
</script>

<style lang="less">
#app {
  height: 100%;
}

.ant-layout {
  height: 100%;
}

.user-name {
  margin-left: 10px;
}

.info-box {
  padding:  0 20px;
  display: inline-flex;
  // min-width: 200px;
  align-items: center;
}

.trigger {
  font-size: 18px;
  line-height: 64px;
  padding: 0 24px;
  cursor: pointer;
  transition: color 0.3s;
}

.trigger:hover {
  color: #1890ff;
}

.logo {
  color: #fff;
  height: 32px;
  background: rgba(255, 255, 255, 0.3);
  margin: 16px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.site-layout .site-layout-background {
  background: #fff;
}
.shadow {
  box-shadow: 0 2px 12px 0 rgb(0 0 0 / 10%);
}
</style>
