<template>
  <div class="demo-autocomplete">
    <tiny-autocomplete
      class="inline-input"
      value-key="name"
      v-model="state1"
      :fetch-suggestions="querySearch"
      placeholder="请输入内容"
    >
      <template #default="{ slotScope }">
        <div>{{ slotScope.name }}</div>
        <span style="font-size: 12px; color: #a29b91">{{ slotScope.address }}</span>
      </template>
    </tiny-autocomplete>
  </div>
</template>

<script setup lang="jsx">
import { ref, onMounted } from 'vue'
import { Autocomplete as TinyAutocomplete } from '@opentiny/vue'

const restaurants = ref([])
const state1 = ref('')
const timeout = ref(null)

onMounted(() => {
  restaurants.value = loadAll()
})

function querySearch(queryString, cb) {
  let results = queryString ? restaurants.value.filter(createFilter(queryString)) : restaurants.value

  // 调用 callback 返回建议列表的数据
  clearTimeout(timeout.value)
  timeout.value = setTimeout(() => {
    cb(results)
  }, 3000 * Math.random())
}

function createFilter(queryString) {
  return (restaurant) => restaurant.name.toLowerCase().indexOf(queryString.toLowerCase()) === 0
}

function loadAll() {
  return [
    {
      name: 'GFD科技YX公司',
      address: '福州'
    },
    {
      name: 'WWWW科技YX公司',
      address: '深圳福田区'
    },
    {
      name: 'RFV有限责任公司',
      address: '中山市'
    },
    {
      name: 'TGBYX公司',
      address: '梅州'
    },
    {
      name: 'YHN科技YX公司',
      address: '韶关'
    },
    {
      name: '康康物业YX公司',
      address: '广州天河区'
    }
  ]
}
</script>

<style scoped>
.demo-autocomplete .tiny-autocomplete {
  width: 270px;
}
</style>
