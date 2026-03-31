<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const BLINK_INDEXES = [2, 4, 8]
const gridRef = ref(null)
let rafId = null
let currentProgress = 0

/**
 * 球的資料結構 — 效能設計：
 *   - 每顆球只需指定 fromCell / toCell（0-8 的格子 index）
 *   - 動畫由 requestAnimationFrame 驅動，只更新 transform，不觸發 layout reflow
 *   - will-change: transform 提示瀏覽器提前建立 GPU layer
 *   - 擴充至 100 顆：直接在此陣列新增物件即可，結構不需改動
 *
 * 動畫執行方式一：CSS @keyframes（格子閃爍，見下方 .bingo-cell--blink）
 * 動畫執行方式二：requestAnimationFrame（球的匯聚動畫，見 renderBalls / handleScroll）
 */
const ballDefs = [
  { id: 'tl', fromCell: 0, toCell: 4 },
  { id: 'tr', fromCell: 2, toCell: 4 },
  { id: 'bl', fromCell: 6, toCell: 4 },
  { id: 'br', fromCell: 8, toCell: 4 },
]

function cellCenter(idx) {
  const cellW = window.innerWidth / 4
  const col = idx % 3
  const row = Math.floor(idx / 3)
  return { x: col * cellW + cellW / 2, y: row * 100 + 50 }
}

function calcStyle(def, progress) {
  const from = cellCenter(def.fromCell)
  const to = cellCenter(def.toCell)
  const x = from.x + (to.x - from.x) * progress - 15
  const y = from.y + (to.y - from.y) * progress - 15
  return { transform: `translate(${x}px, ${y}px)` }
}

// 初始位置：progress = 0，球在各自角落格子的中心
const ballStyles = ref(ballDefs.map(def => calcStyle(def, 0)))

function renderBalls() {
  ballDefs.forEach((def, i) => {
    ballStyles.value[i] = calcStyle(def, currentProgress)
  })
  rafId = null
}

function handleScroll() {
  if (!gridRef.value) return
  const rect = gridRef.value.getBoundingClientRect()
  currentProgress = rect.top < 0 ? Math.min(Math.abs(rect.top) / 300, 1) : 0
  if (!rafId) rafId = requestAnimationFrame(renderBalls)
}

onMounted(() => window.addEventListener('scroll', handleScroll, { passive: true }))
onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  if (rafId) cancelAnimationFrame(rafId)
})
</script>

<template>
  <div class="grid-wrapper" ref="gridRef">
    <!-- 動畫方式一：CSS @keyframes — 指定格子持續閃爍 -->
    <div class="grid-cells">
      <div
        v-for="(_, idx) in 9"
        :key="idx"
        class="bingo-cell"
        :class="{ 'bingo-cell--blink': BLINK_INDEXES.includes(idx) }"
      />
    </div>

    <!-- 動畫方式二：requestAnimationFrame — 四顆球朝中心格匯聚 -->
    <div class="grid-balls">
      <span
        v-for="(def, i) in ballDefs"
        :key="def.id"
        class="ball"
        :style="ballStyles[i]"
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

/* 球層：absolute，不繼承格子的 opacity */
.grid-balls {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 10;
}

.ball {
  position: absolute;
  top: 0;
  left: 0;
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: #a5f12b;
  will-change: transform;
}
</style>
