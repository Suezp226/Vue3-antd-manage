<template>
    <a-menu class="menu" v-model:selectedKeys="selectedKeys" theme="dark" mode="inline" @click="clickMenu">
      <a-menu-item key="home">
        <user-outlined />
        <span>Home</span>
      </a-menu-item>
      <a-menu-item key="about">
        <video-camera-outlined />
        <span>About</span>
      </a-menu-item>
    </a-menu>
</template>

<script setup lang="ts">
    import {
    UserOutlined,
    VideoCameraOutlined,
    } from '@ant-design/icons-vue';
    import { ref } from 'vue'
    const emit = defineEmits(['clickMenu'])

    const clickMenu = function(event: any) {
        let { item,key,keyPath } = event
        emit('clickMenu',{key:key})
    }

    
</script>

<style lang="less">
   
</style>