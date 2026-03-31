<script setup>
defineProps({
  item: { type: Object, required: true },
  activeKey: { type: String, default: null },
  expandedKeys: { type: Array, required: true },
  depth: { type: Number, default: 0 },
})

defineEmits(['toggle'])
</script>

<template>
  <li>
    <button
      class="menu-item"
      :class="{ 'menu-item--active': activeKey === item.key }"
      :style="{ paddingLeft: `${16 + depth * 14}px` }"
      @click="$emit('toggle', item)"
    >
      <span>{{ item.text }}</span>
      <span
        v-if="item.children?.length"
        class="menu-arrow"
        :class="{ 'menu-arrow--open': expandedKeys.includes(item.key) }"
      >›</span>
    </button>

    <Transition name="expand">
      <ul v-if="item.children?.length && expandedKeys.includes(item.key)" class="menu-list">
        <!-- 遞迴渲染，最多支援任意層數（含需求的 100 層） -->
        <MenuItem
          v-for="child in item.children"
          :key="child.key"
          :item="child"
          :active-key="activeKey"
          :expanded-keys="expandedKeys"
          :depth="depth + 1"
          @toggle="$emit('toggle', $event)"
        />
      </ul>
    </Transition>
  </li>
</template>

<style scoped>
.menu-list {
  list-style: none;
  margin: 0;
  padding: 0;
}

.menu-item {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 11px;
  padding-bottom: 11px;
  padding-right: 16px;
  background: none;
  border: none;
  text-align: left;
  font-size: 14px;
  color: #2a2a2a;
  cursor: pointer;
  transition: background 0.15s;
}

.menu-item:active {
  background: #f0f0f0;
}

.menu-item--active {
  background: #f3eef0;
  color: #71515f;
  font-weight: 600;
}

.menu-arrow {
  font-size: 18px;
  color: #aaa;
  line-height: 1;
  transition: transform 0.2s ease;
  display: inline-block;
  flex-shrink: 0;
}

.menu-arrow--open {
  transform: rotate(90deg);
}

.expand-enter-active,
.expand-leave-active {
  transition: opacity 0.2s ease, max-height 0.3s ease;
  overflow: hidden;
  max-height: 2000px;
}
.expand-enter-from,
.expand-leave-to {
  opacity: 0;
  max-height: 0;
}
</style>
