<script setup>
defineProps({
  item: { type: Object, required: true },
})
</script>

<template>
  <li>
    <button class="menu-item">
      <span>{{ item.text }}</span>
    </button>
  </li>
</template>

<style scoped>
.menu-item {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 11px 16px;
  background: none;
  border: none;
  text-align: left;
  font-size: 14px;
  color: #2a2a2a;
  cursor: pointer;
}
</style>
