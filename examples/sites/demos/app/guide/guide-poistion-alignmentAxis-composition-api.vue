<template>
  <div>
    <div class="guide-title">
      <tiny-button plain @click="stepStart1">新手引导1</tiny-button>
      <tiny-button plain @click="stepStart2">新手引导2</tiny-button>
      <tiny-button plain @click="stepStart3">新手引导3</tiny-button>
    </div>
    <div class="guide-content">
      <tiny-input class="a1" v-model="input" placeholder="请输入内容"></tiny-input>
      <tiny-input class="a2" v-model="input" placeholder="请输入内容"></tiny-input>
      <tiny-input class="a3" v-model="input" placeholder="请输入内容"></tiny-input>
    </div>
    <tiny-guide :show-step="showStep1" :dom-data="adata1" :alignment-axis="10" :pop-position="popPosition"></tiny-guide>
    <tiny-guide :show-step="showStep2" :dom-data="adata2" :alignment-axis="20" :pop-position="popPosition"></tiny-guide>
    <tiny-guide :show-step="showStep3" :dom-data="adata3" :alignment-axis="50" :pop-position="popPosition"></tiny-guide>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import { Guide as TinyGuide, Button as TinyButton, Input as TinyInput } from '@opentiny/vue'

const showStep1 = ref(false)
const showStep2 = ref(false)
const showStep3 = ref(false)
const input = ref('')
const popPosition = ref('top-end')
const adata1 = ref([
  {
    title: '新手引导标题1',
    text: '这里是新手引导文案这里是新手引导文案这里是新手引导文案这里是新手引导文案这里是新手引导文案这里是新手引导文案',
    domElement: '.a1',
    button: [
      {
        text: '下一步',
        action: 'next'
      }
    ]
  }
])
const adata2 = ref([
  {
    title: '新手引导标题2',
    text: '这里是新手引导文案这里是新手引导文案这里是新手引导文案这里是新手引导文案这里是新手引导文案这里是新手引导文案',
    domElement: '.a2',
    button: [
      {
        text: '下一步',
        action: 'next'
      }
    ]
  }
])
const adata3 = ref([
  {
    title: '新手引导标题3',
    text: '这里是新手引导文案这里是新手引导文案这里是新手引导文案这里是新手引导文案这里是新手引导文案这里是新手引导文案',
    domElement: '.a3',
    button: [
      {
        text: '下一步',
        action: 'next'
      }
    ]
  }
])

function stepStart1() {
  showStep1.value = !showStep1.value
}

function stepStart2() {
  showStep2.value = !showStep2.value
}

function stepStart3() {
  showStep3.value = !showStep3.value
}
</script>

<style scoped>
.guide-title {
  display: flex;
  justify-content: space-around;
}
.guide-content {
  display: flex;
  justify-content: space-around;
}
</style>
