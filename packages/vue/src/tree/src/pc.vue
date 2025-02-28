<!--
 * Copyright (c) 2022 - present TinyVue Authors.
 * Copyright (c) 2022 - present Huawei Cloud Computing Technologies Co., Ltd.
 *
 * Use of this source code is governed by an MIT-style license.
 *
 * THE OPEN SOURCE SOFTWARE IN THIS PRODUCT IS DISTRIBUTED IN THE HOPE THAT IT WILL BE USEFUL,
 * BUT WITHOUT ANY WARRANTY, WITHOUT EVEN THE IMPLIED WARRANTY OF MERCHANTABILITY OR FITNESS FOR
 * A PARTICULAR PURPOSE. SEE THE APPLICABLE LICENSES FOR MORE DETAILS.
 *
 -->

<template>
  <div
    class="tiny-tree"
    :class="{
      'tiny-tree--highlight-current': highlightCurrent,
      'is-dragging': !!state.dragState.draggingNode,
      'is-drop-not-allow': !state.dragState.allowDrop,
      'is-drop-inner': state.dragState.dropType === 'inner',
      'tiny-tree--small': size === 'small'
    }"
    role="tree"
  >
    <!-- 树视图 -->
    <template v-if="viewType === 'tree'">
      <tree-node
        v-for="child in state.root.childNodes"
        :action="state.action"
        :show-radio="showRadio"
        :theme="theme"
        :show-number="showNumber"
        :collapsible="collapsible"
        :node-height="nodeHeight"
        :current-radio="state.currentRadio"
        @radio-change="state.currentRadio.value = $event"
        :expand-icon="expandIcon"
        :shrink-icon="shrinkIcon"
        :expand-icon-color="expandIconColor"
        :shrink-icon-color="shrinkIconColor"
        :node="child"
        :props="props"
        :render-after-expand="renderAfterExpand"
        :show-checkbox="showCheckbox"
        :key="getNodeKey(child)"
        :node-key="nodeKey"
        :render-content="renderContent"
        @node-expand="handleNodeExpand"
        :check-easily="state.checkEasily"
        :show-line="showLine"
      >
        <template #prefix="slotScoped"><slot name="prefix" :node="slotScoped.node"></slot></template>
        <template #suffix="slotScoped"><slot name="suffix" :node="slotScoped.node"></slot></template>
        <template #operation="slotScoped"><slot name="operation" :node="slotScoped.node"></slot></template>
      </tree-node>
    </template>

    <!-- 平铺视图 -->
    <template v-else-if="viewType === 'plain'">
      <div
        v-for="(plainNode, i) in state.plainNodeStore.filteredNodes"
        :key="`${plainNode.nodeKeyPath}-${i}`"
        class="tiny-tree__plain-node"
      >
        <div class="tiny-tree__plain-node-title">
          <tiny-checkbox
            :modelValue="plainNode.node.checked"
            :indeterminate="plainNode.node.indeterminate"
            :disabled="!!plainNode.node.disabled"
            @change="handleCheckPlainNode($event, plainNode)"
          ></tiny-checkbox>
          <span class="tiny-tree__plain-node-title-txt">{{ plainNode.title }}</span>
          <span class="tiny-tree__plain-node-title-loc">
            <icon-mark-on @click="handleClickPlainNode($event, plainNode)"></icon-mark-on>
          </span>
        </div>
        <div v-if="plainNode.auxi" class="tiny-tree__plain-node-auxi">
          <div>{{ plainNode.auxi }}</div>
        </div>
      </div>
    </template>

    <div class="tiny-tree__empty-block" v-if="state.isEmpty">
      <slot name="empty">
        <span class="tiny-tree__empty-text">{{ state.showEmptyText }}</span>
      </slot>
    </div>
    <div v-show="state.dragState.showDropIndicator" class="tiny-tree__drop-indicator" ref="dropIndicator"></div>
    <div class="tiny-tree__strict-bottom" v-if="state.store.checkStrictly && showCheckEasily">
      <div class="tiny-tree__strict-bottom-content">
        <tiny-switch mini @change="switchToggle" v-model="state.checkEasily"></tiny-switch>
        <div class="tiny-tree__bottom-text">
          <slot name="switchText">
            <span>{{ t('ui.tree.switchText') }}</span>
          </slot>
        </div>
      </div>
    </div>

    <tiny-popover
      ref="deletePopover"
      v-model="state.action.popoverVisible"
      popper-class="tiny-tree__del-popover"
      placement="top"
      :width="state.action.isLeaf ? 200 : 270"
      trigger="manual"
      :reference="state.action.referenceElm"
    >
      <div class="tiny-tree__del-body" v-clickoutside="cancelDelete">
        <div class="tiny-tree__del-icon">
          <icon-warning></icon-warning>
        </div>
        <div class="tiny-tree__del-content">
          <template v-if="state.action.isLeaf">
            <div>{{ t('ui.tree.deleteTip') }}</div>
          </template>
          <template v-else>
            <div>{{ t('ui.tree.preserveSubnodeTip') }}</div>
            <div class="tiny-tree__del-checkbox">
              <tiny-checkbox v-model="state.action.isSaveChildNode" ref="deleteCheckbox">{{
                t('ui.tree.preserveSubnodeData')
              }}</tiny-checkbox>
            </div>
          </template>
        </div>
      </div>
      <div class="tiny-tree__del-footer">
        <tiny-button @click="cancelDelete">{{ t('ui.base.cancel') }}</tiny-button>
        <tiny-button type="primary" @click="deleteAction">{{ t('ui.base.confirm') }}</tiny-button>
      </div>
    </tiny-popover>
  </div>
</template>

<script lang="tsx">
import { renderless, api } from '@opentiny/vue-renderless/tree/vue'
import { props, setup, defineComponent, directive } from '@opentiny/vue-common'
import { iconWarning, iconMarkOn } from '@opentiny/vue-icon'
import Switch from '@opentiny/vue-switch'
import Popover from '@opentiny/vue-popover'
import Button from '@opentiny/vue-button'
import Checkbox from '@opentiny/vue-checkbox'
import Clickoutside from '@opentiny/vue-renderless/common/deps/clickoutside'
import TreeNode from './tree-node.vue'

export default defineComponent({
  directives: directive({ Clickoutside }),
  props: [
    ...props,
    'data',
    'dataset',
    'parentKey',
    'expandIcon',
    'shrinkIcon',
    'expandIconColor',
    'shrinkIconColor',
    'emptyText',
    'renderAfterExpand',
    'nodeKey',
    'checkStrictly',
    'defaultExpandAll',
    'expandOnClickNode',
    'checkOnClickNode',
    'checkDescendants',
    'autoExpandParent',
    'defaultCheckedKeys',
    'defaultExpandedKeys',
    'defaultExpandedKeysHighlight',
    'currentNodeKey',
    'renderContent',
    'showCheckbox',
    'draggable',
    'allowDrag',
    'allowDrop',
    'props',
    'lazy',
    'highlightCurrent',
    'load',
    'filterNodeMethod',
    'accordion',
    'indent',
    'icon',
    'iconTriggerClickNode',
    'showRadio',
    'showContextmenu',
    'showNumber',
    'collapsible',
    'nodeHeight',
    'showCheckEasily',
    'size',
    'deleteDisabledKeys',
    'editDisabledKeys',
    'addDisabledKeys',
    'theme',
    'viewType',
    'pathSplit',
    'filterPlainMethod',
    'afterLoad',
    'lazyCurrent',
    'baseIndent',
    'showLine',
  ],
  components: {
    TreeNode,
    IconWarning: iconWarning(),
    IconMarkOn: iconMarkOn(),
    TinyPopover: Popover,
    TinyCheckbox: Checkbox,
    TinyButton: Button,
    TinySwitch: Switch
  },
  emits: [
    'node-expand',
    'node-drag-leave',
    'check-change',
    'check',
    'node-drag-over',
    'node-drag-enter',
    'node-drag-start',
    'node-drag-end',
    'node-drop',
    'current-change',
    'node-click',
    'leave-plain-view',
    'check-plain'
  ],
  provide() {
    return {
      parentTree: this,
      TreeRoot: this
    }
  },
  setup(props, context) {
    return setup({ props, context, renderless, api })
  }
})
</script>
