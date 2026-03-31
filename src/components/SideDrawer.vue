<script setup>
import { ref, computed } from 'vue'
import menuData from '../data/menuData.js'
import MenuItem from './MenuItem.vue'

defineProps({ open: Boolean })
const emit = defineEmits(['close'])

const activeKey = ref(null)
const expandedKeys = ref([])

function findItemByKey(key, nodes) {
  for (const node of nodes) {
    if (node.key === key) return node
    if (node.children) {
      const found = findItemByKey(key, node.children)
      if (found) return found
    }
  }
  return null
}

// 找同層節點（用來收起同層其他展開的項目）
function findSiblings(key, nodes) {
  for (const node of nodes) {
    if (node.children) {
      if (node.children.some(c => c.key === key)) return node.children
      const found = findSiblings(key, node.children)
      if (found) return found
    }
  }
  return null
}

// 收起某節點及其所有子孫
function collapseSubtree(item) {
  expandedKeys.value = expandedKeys.value.filter(k => k !== item.key)
  item.children?.forEach(child => collapseSubtree(child))
}

function handleToggle(item) {
  activeKey.value = item.key

  if (!item.children?.length) return

  if (expandedKeys.value.includes(item.key)) {
    collapseSubtree(item)
  } else {
    // 收起同層其他展開的節點（root 層的 siblings 就是 menuData）
    const siblings = findSiblings(item.key, menuData) ?? menuData
    siblings.forEach(s => {
      if (s.key !== item.key) collapseSubtree(s)
    })
    expandedKeys.value.push(item.key)
  }
}

// 將樹狀資料攤平為下拉選單用的列表
function flattenTree(nodes, depth = 0) {
  return nodes.flatMap(node => [
    { key: node.key, text: '　'.repeat(depth) + node.text },
    ...(node.children ? flattenTree(node.children, depth + 1) : []),
  ])
}
const flatItems = computed(() => flattenTree(menuData))

function handleSelectChange(e) {
  const item = findItemByKey(e.target.value, menuData)
  if (item) handleToggle(item)
}
</script>

<template>
  <!-- 遮罩 -->
  <Transition name="fade">
    <div v-if="open" class="drawer-backdrop" @click="emit('close')" />
  </Transition>

  <!-- 抽屜 -->
  <Transition name="slide">
    <div v-if="open" class="drawer" @click.stop>
      <div class="drawer-header">
        <span class="drawer-title">選單</span>
        <button class="drawer-close" @click="emit('close')">✕</button>
      </div>

      <!-- 下拉選單：包含所有項目，選取等同點擊該項目 -->
      <div class="select-wrap">
        <select
          class="category-select"
          :value="activeKey || ''"
          @change="handleSelectChange"
        >
          <option value="" disabled>選擇項目</option>
          <option v-for="item in flatItems" :key="item.key" :value="item.key">
            {{ item.text }}
          </option>
        </select>
      </div>

      <!-- 階層選單：遞迴元件，支援任意深度（含 100 層需求） -->
      <nav class="drawer-nav">
        <ul class="menu-list">
          <MenuItem
            v-for="item in menuData"
            :key="item.key"
            :item="item"
            :active-key="activeKey"
            :expanded-keys="expandedKeys"
            :depth="0"
            @toggle="handleToggle"
          />
        </ul>
      </nav>
    </div>
  </Transition>
</template>

<style scoped>
.drawer-backdrop {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.45);
  z-index: 100;
}

.drawer {
  position: fixed;
  top: 0;
  left: 0;
  width: 75vw;
  max-width: 300px;
  height: 100%;
  background: #fff;
  z-index: 101;
  display: flex;
  flex-direction: column;
  box-shadow: 4px 0 16px rgba(0, 0, 0, 0.15);
}

.drawer-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 20px 16px 16px;
  border-bottom: 1px solid #eee;
  flex-shrink: 0;
}

.drawer-title {
  font-size: 17px;
  font-weight: 600;
  color: #1a1a1a;
}

.drawer-close {
  background: none;
  border: none;
  font-size: 18px;
  color: #666;
  cursor: pointer;
  padding: 4px;
  line-height: 1;
}

.select-wrap {
  padding: 12px 16px;
  border-bottom: 1px solid #eee;
  flex-shrink: 0;
}

.category-select {
  width: 100%;
  padding: 8px 10px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 14px;
  font-family: inherit;
  color: #2a2a2a;
  background: #fff;
}

.drawer-nav {
  flex: 1;
  overflow-y: auto;
  padding: 8px 0;
}

.menu-list {
  list-style: none;
  margin: 0;
  padding: 0;
}

/* 過場動畫 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}

.slide-enter-active,
.slide-leave-active {
  transition: transform 0.28s ease;
}
.slide-enter-from,
.slide-leave-to {
  transform: translateX(-100%);
}
</style>
