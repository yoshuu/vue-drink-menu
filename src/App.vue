<script setup>
import { ref } from 'vue'
import BingoGrid from './components/BingoGrid.vue'
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
      <section class="grid-section">
        <h2 class="section-title">今日推薦</h2>
        <div class="grid-container">
          <BingoGrid />
        </div>
      </section>

      <!-- 捲動測試用的空白區域 -->
      <section class="filler-section">
        <p class="filler-text">往下滑看看圓點的變化</p>
      </section>
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

.section-title {
  font-size: 15px;
  font-weight: 500;
  color: #666;
  margin: 0 0 20px;
  letter-spacing: 0.5px;
}

.grid-container {
  display: flex;
  justify-content: center;
  padding: 20px 0;
}

.filler-section {
  margin-top: 60px;
  height: 900px;
  display: flex;
  align-items: flex-start;
  justify-content: center;
}

.filler-text {
  font-size: 13px;
  color: #ccc;
  letter-spacing: 0.8px;
  padding-top: 8px;
}
</style>
