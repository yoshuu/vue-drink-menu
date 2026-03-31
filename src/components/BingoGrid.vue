<script setup>
const BLINK_INDEXES = [2, 4, 8]
</script>

<template>
  <div class="grid-wrapper">
    <!-- 動畫方式一：CSS @keyframes — 指定格子持續閃爍 -->
    <div class="grid-cells">
      <div
        v-for="(_, idx) in 9"
        :key="idx"
        class="bingo-cell"
        :class="{ 'bingo-cell--blink': BLINK_INDEXES.includes(idx) }"
      />
    </div>
  </div>
</template>

<style scoped>
.grid-wrapper {
  position: relative;
  display: inline-block;
}

.grid-cells {
  display: grid;
  grid-template-columns: repeat(3, calc(100vw / 4));
}

.bingo-cell {
  width: calc(100vw / 4);
  height: 100px;
  border: 2px solid black;
  background: radial-gradient(circle, rgba(113, 81, 95, 1) 81%, rgba(0, 0, 0, 1) 100%);
  box-sizing: border-box;
}

/* 動畫方式一：CSS @keyframes 淡入淡出閃爍 */
.bingo-cell--blink {
  animation: cell-blink 1.4s ease-in-out infinite;
}

@keyframes cell-blink {
  0%   { opacity: 1; }
  40%  { opacity: 0.6; }
  60%  { opacity: 0.6; }
  100% { opacity: 1; }
}
</style>
