<template>
  <div class="demo-image__lazy" style="height: 300px; overflow-y: auto">
    <tiny-image v-for="url in srcList" :key="url" :src="url" lazy></tiny-image>
  </div>
</template>

<script setup lang="jsx">
import { ref } from 'vue'
import { Image as TinyImage } from '@opentiny/vue'

const srcList = ref([
  `${import.meta.env.VITE_APP_BUILD_BASE_URL}static/images/mountain.png`,
  `${import.meta.env.VITE_APP_BUILD_BASE_URL}static/images/book.jpg`,
  `${import.meta.env.VITE_APP_BUILD_BASE_URL}static/images/bridge.jpg`
])
</script>

<style scoped>
.demo-image__lazy .tiny-image {
  display: block;
  min-height: 200px;
}
</style>
