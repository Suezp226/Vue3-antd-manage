<template>
    <div> Home </div>
    <div> {{ name }} </div>
</template>


<script setup lang="ts">
import { ref } from 'vue'

defineProps<{ msg: string }>()

const count = ref(0)
let name = 20;

</script>

<style lang="less">
    
</style>