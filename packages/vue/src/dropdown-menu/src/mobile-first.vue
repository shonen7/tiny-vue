<template>
  <transition name="tiny-zoom-in-top" :duration="150" @after-leave="doDestroy">
    <ul
      data-tag="tiny-dropdown-menu"
      v-show="state.showPopper"
      :class="
        m(
          'min-w-[theme(spacing.18)] max-w-[theme(spacing.52)] absoulte bg-color-bg-1',
          { 'py-1.5': state.size === 'medium' },
          { 'py-1.5': state.size === 'small' },
          { 'py-0.75': state.size === 'mini' },
          { 'overflow-x-hidden scrollbar-size-0': maxHeight },
          multiStage ? '!block  rounded-none' : 'rounded sm:my-1 py-1 my-2 shadow-sm sm:shadow-md',
          popperClass
        )
      "
      :style="`max-height:${maxHeight}px`"
    >
      <tiny-tooltip
        ref="tooltip"
        effect="light"
        :content="state.label"
        placement="top-start"
        :manual="true"
        v-model="state.showContent"
      >
      </tiny-tooltip>
      <slot :selected-index="state.selectedIndex"></slot>
    </ul>
  </transition>
</template>

<script lang="ts">
import { renderless, api } from '@opentiny/vue-renderless/dropdown-menu/vue'
import { props, setup, defineComponent } from '@opentiny/vue-common'
import Tooltip from '@opentiny/vue-tooltip'

export default defineComponent({
  props: [
    ...props,
    'visibleArrow',
    'arrowOffset',
    'placement',
    'popperClass',
    'popperAppendToBody',
    'checkedStatus',
    'multiStage',
    'maxHeight'
  ],
  emits: ['menu-item-click', 'created'],
  components: {
    TinyTooltip: Tooltip
  },
  setup(props, context): any {
    return setup({ props, context, renderless, api })
  }
})
</script>
