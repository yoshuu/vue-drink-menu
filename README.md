# vue-drink-menu

飲品選單 — Vue 3 前端作業

已經部署到 vercel 上面：https://vue-drink-menu.vercel.app/

## 技術選型

- Vue 3 Composition API（`<script setup>`）
- Vite
- 純 CSS，無 UI 框架

## 功能說明

### Hamburger 按鈕 + 側邊抽屜

點擊左上角按鈕開啟側邊選單，點擊遮罩或 ✕ 關閉。開關動畫使用 Vue `<Transition>`（fade / slide）。

### 九宮格動畫

包含兩種動畫實作方式：

1. **CSS `@keyframes`**：右上、正中、右下三格持續閃爍（opacity 淡入淡出）
2. **`requestAnimationFrame`**：四顆球隨頁面捲動從四個角落向中心匯聚，透過 `getBoundingClientRect()` 計算進度，只更新 `transform` 避免觸發 layout reflow，並以 `will-change: transform` 提示瀏覽器建立 GPU layer

兩層 DOM 分離（`.grid-cells` / `.grid-balls` absolute + z-index），避免 opacity 繼承影響球的顯示。

### 階層選單

- 遞迴元件 `MenuItem.vue`，支援任意深度巢狀
- 每層同時只展開一個節點（accordion 行為），收合時同步收合所有子孫
- 選取項目高亮顯示
- 下拉 `<select>` 與樹狀選單雙向同步
- 選取狀態透過 `localStorage` 跨頁面保留，重新開啟時自動展開至上次選取的節點

## 啟動方式

```bash
npm install
npm run dev
```
