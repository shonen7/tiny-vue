<template>
  <tiny-select
    v-model="value"
    ref="select"
    filterable
    remote
    reserve-keyword
    placeholder="请输入关键词"
    :remote-method="remoteMethod"
    :loading="loading"
    loading-text="Loading..."
  >
    <tiny-option v-for="item in options" :key="item.value" :label="item.label" :value="item.value"> </tiny-option>
  </tiny-select>
</template>

<script lang="jsx">
import { Select, Option } from '@opentiny/vue'

export default {
  components: {
    TinySelect: Select,
    TinyOption: Option
  },
  data() {
    return {
      loading: false,
      value: '',
      options: [],
      list: [],
      states: [
        'Alabama',
        'Alaska',
        'Arizona',
        'Arkansas',
        'California',
        'Colorado',
        'Connecticut',
        'Delaware',
        'Florida',
        'Georgia',
        'Hawaii',
        'Idaho',
        'Illinois',
        'Indiana',
        'Iowa',
        'Kansas',
        'Kentucky',
        'Louisiana',
        'Maine',
        'Maryland',
        'Massachusetts',
        'Michigan',
        'Minnesota',
        'Mississippi',
        'Missouri',
        'Montana',
        'Nebraska',
        'Nevada',
        'New Hampshire',
        'New Jersey',
        'New Mexico',
        'New York',
        'North Carolina',
        'North Dakota',
        'Ohio',
        'Oklahoma',
        'Oregon',
        'Pennsylvania',
        'Rhode Island',
        'South Carolina',
        'South Dakota',
        'Tennessee',
        'Texas',
        'Utah',
        'Vermont',
        'Virginia',
        'Washington',
        'West Virginia',
        'Wisconsin',
        'Wyoming'
      ]
    }
  },
  mounted() {
    setTimeout(() => {
      this.$refs.select.state.previousQuery = undefined
    }),
      (this.list = this.states.map((item) => {
        return { value: item, label: item }
      }))
  },
  methods: {
    remoteMethod(query) {
      if (query !== undefined) {
        this.loading = true
        setTimeout(() => {
          this.loading = false
          this.options = this.list.filter((item) => {
            return item.label.toLowerCase().includes(query.toLowerCase())
          })
        }, 200)
      } else {
        this.options = []
      }
    }
  }
}
</script>
