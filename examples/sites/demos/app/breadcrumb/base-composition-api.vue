<template>
  <tiny-breadcrumb @select="breadcrumbClick">
    <tiny-breadcrumb-item :to="{ path: '/' }" @select="breadcrumbItemClick" label="首页"></tiny-breadcrumb-item>
    <tiny-breadcrumb-item>
      <a href="/"> 产品 </a>
    </tiny-breadcrumb-item>
    <tiny-breadcrumb-item :to="{ path: '/breadcrumb' }"> 软件 </tiny-breadcrumb-item>
  </tiny-breadcrumb>
</template>

<script setup lang="jsx">
import { Breadcrumb as TinyBreadcrumb, BreadcrumbItem as TinyBreadcrumbItem, Notify } from '@opentiny/vue'

function breadcrumbClick(value) {
  Notify({
    type: 'info',
    title: 'tiny-breadcrumb 触发选择面包屑事件',
    message: JSON.stringify(value),
    position: 'top-right',
    duration: 3000
  })
}

function breadcrumbItemClick(value) {
  Notify({
    type: 'info',
    title: 'tiny-breadcrumb-item 触发选择面包屑事件',
    message: JSON.stringify(value),
    position: 'top-right',
    duration: 3000
  })
}
</script>
