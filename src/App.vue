<script setup>
import { ref } from 'vue'
import SideDrawer from './components/SideDrawer.vue'

const drawerOpen = ref(false)
</script>

<template>
  <div class="page">
    <header class="top-bar">
      <button class="menu-btn" @click="drawerOpen = true">
        <span class="menu-btn__line" />
        <span class="menu-btn__line" />
        <span class="menu-btn__line" />
      </button>
      <span class="top-bar__title">飲品選單</span>
    </header>

    <main class="main-content">
    </main>

    <SideDrawer :open="drawerOpen" @close="drawerOpen = false" />
  </div>
</template>

<style scoped>
.page {
  width: 100%;
  max-width: 390px;
  margin: 0 auto;
  min-height: 100vh;
  background: #fafafa;
  position: relative;
}

.top-bar {
  position: sticky;
  top: 0;
  z-index: 50;
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 14px 16px;
  background: #fff;
  border-bottom: 1px solid #e8e8e8;
}

.menu-btn {
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 24px;
  height: 18px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0;
  flex-shrink: 0;
}

.menu-btn__line {
  display: block;
  width: 100%;
  height: 2px;
  background: #2a2a2a;
  border-radius: 2px;
}

.top-bar__title {
  font-size: 17px;
  font-weight: 600;
  color: #1a1a1a;
}

.main-content {
  padding: 24px 16px;
}
</style>
