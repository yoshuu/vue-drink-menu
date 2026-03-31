<script setup></script>

<template>
  <div class="page">
    <header class="top-bar">
      <span class="top-bar__title">飲品選單</span>
    </header>
    <main class="main-content">
    </main>
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
  display: flex;
  align-items: center;
  gap: 14px;
  padding: 14px 16px;
  background: #fff;
  border-bottom: 1px solid #e8e8e8;
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
