---
theme: seriph
background: https://i.ytimg.com/vi/5CXQDCEZtPc/sddefault.jpg
title: 《React 思維進化》簡報
info: |
  ## 關於這份簡報
  嗨！我是上竣~

  這是[《React 思維進化》讀書會](https://github.com/Tech-Book-Community/Zet-React-Book)的簡報

  原始檔都放在[Github](https://github.com/CK642509/slidev_react)上，歡迎查看
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
---

# React 思維進化讀書會

## 2-7 ~ 2-9

2025.03.18

---

# 上竣
- 非本科系 (農業化學系、生醫電資所)
- ~ 3 年年資
- AI 新創公司，全端開發 (Vue + Python)
- iThome 鐵人賽
  - 2024 Python 組 「用 Python 打造你的 Discord BOT」 佳作
  - 2023 Software Development 組 「FastAPI 開發筆記：從新手到專家的成長之路」

---

# 前情提要

- DOM vs. vDOM
- React Element
- JSX
- 單向資料流 與 一律重繪

<br><br><br>

<v-click>

<div class="text-center">

## Component

</div>

</v-click>

<br>

<v-click>

<div class="text-center">

### 2-7 畫面組裝的藍圖：component 初探
### 2-8 React 畫面更新的發動機：state 初探
### 2-9 React 畫面更新的流程機制：reconciliation

</div>

</v-click>

<!-- 
- [click] 導入一個新的概念，叫做 component
- [click] 在 2-7 會簡單介紹什麼是 component，以及如何透過 props，將外部參數傳入到 component 內
- 接著，在 2-8 會介紹什麼是 state，以及在 component 內，如何用 state 做資料狀態管理與畫面更新
- 最後，在 2-9 會以 component 的角度，再完整地走過一次 React 畫面更新的流程機制 -->


---
class: flex justify-center items-center

---

# 2-7 畫面組裝的藍圖：component 初探

---
layout: two-cols
---

# 什麼是 Component

<v-click>

- 由開發者自定義的畫面元件藍圖
- 可重用的程式碼片段

</v-click>

<v-click>

## 抽象化
- 根據需求，將關心的特徵與行為歸納出來
- 將實作細節或複雜性封裝在內部
- 通用性、重用性

</v-click>

::right::

<br><br><br><br><br><br>

<v-click>

![](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*wmRD7YIgQsWEt8PBa1dIyQ.png)
<span class="opacity-30 text-xs">https://sam-j.medium.com/react-component-structure-d38d59eefffd</span>

</v-click>


---

# 定義 Component




---
class: flex justify-center items-center

---

# Thank you