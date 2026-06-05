# 查經週訓 HTML 製作 Prompt Template

## 你的任務

將查經聚會的逐字稿或筆記，整理成一份視覺化的 HTML 週訓頁面。
風格參考：`weekly-1221-20260531.html`（已提供的範本）。

---

## 整體風格規範

- 字型：`Noto Serif TC`（標題）、`Noto Sans TC`（內文），從 Google Fonts 引入
- 配色系統（CSS 變數，直接複製到 `:root`）：

```css
--gold: #C9A84C;  --gold-light: #E8D08A;  --gold-pale: #FAF3DC;
--deep: #14100C;  --ink: #28200E;         --ink-soft: #3A3020;
--muted: #7A6B50; --border: #E0D4BA;      --parchment: #FBF7F0;
--parchment2: #F4EDD8;
--dove: #5C7289;  --dove-light: #8CAAC4;  --dove-pale: #EAF1F8;
--ruby: #8C2030;  --ruby-light: #C44058;  --ruby-pale: #FAEAED;
--teal: #2E7A70;  --teal-light: #4BADA0;  --teal-pale: #E0F2EF;
--sage: #4E6848;  --sage-light: #78A070;  --sage-pale: #EAF3E6;
--r: 14px; --r-lg: 22px;
--shadow: 0 2px 16px rgba(20,16,12,.07);
--shadow-hover: 0 8px 36px rgba(20,16,12,.14);
```

- 背景色：`var(--parchment)`，整體質感像羊皮紙
- 五種主題色可交替用在不同 Section：**dove（藍灰）/ teal（青綠）/ gold（金）/ ruby（深紅）/ sage（鼠尾草綠）**

---

## 頁面固定結構（每份都要有）

### 1. Hero 區塊
```html
<header class="hero">
  <div class="hero-series">📖 {輯號} 輯週訓</div>
  <h1>{主標題前段}<em>{關鍵字}</em>{主標題後段}</h1>
  <!-- em 用 gold-light 色高亮，選一兩個關鍵字放進去 -->
  <p class="hero-meta">{日期} {聚會類型} · 主領：{主領姓名}</p>
  <div class="hero-date-badge">週訓整理日期 {整理日期}</div>
</header>
```

### 2. 黏性導覽列（TOC）
- 背景 `var(--deep)`，置頂 sticky
- 每個 Section 對應一個 `toc-link`，顏色用那個 Section 的主題色
- JS 監聽捲動，自動高亮目前區塊（active class + gold border-bottom）

### 3. Overview 總覽
- 深色背景的「核心信息摘要」（2–3 個信息，網格排列）
- 速覽卡片（`overview-card`），點擊跳到對應 Section
- 卡片數量 = Section 數量，通常 4–6 張

### 4. 主內容 Sections（數量依內容決定，通常 4–6 個）
- 每個 Section 有 `sec-anchor`（錨點）+ `sec-hdr`（標題列含 badge）
- Section 之間用 `divider` 分隔線
- 結構參考下方「可用元件庫」

### 5. Footer
```html
<footer class="page-footer">
  <p>{輯號} 輯週訓 · <strong>{主題}</strong></p>
  <p>{日期} {聚會類型} · 主領：{主領} · 週訓整理日期 {整理日期}</p>
</footer>
```

---

## 可用元件庫（依內容需要自由組合）

### qblock — 核心引言／禱告
> 深色背景大引號風格，適合：核心經文、開場引言、結束禱告
```html
<div class="qblock">
  <p>前段文字<em>強調語句</em>後段文字</p>
  <p class="qblock-src">— 出處</p>
</div>
```

---

### hbox — 重點框
> 左邊框色條，適合：觀點說明、提醒、歸納重點
> 顏色選 `gold / dove / ruby / teal / sage`
```html
<div class="hbox {顏色}">
  <div class="hbox-label">小標標籤</div>
  <h3>框內標題</h3>
  <p>說明文字</p>
  <ul><li>條列</li></ul>  <!-- 選用 -->
</div>
```

---

### vision-list — 步驟 / 要點清單
> 圓形編號 + 說明，適合：論點展開、時間軸、分段講解
> `.v-num` 顏色選 `dove / teal / gold / ruby / sage`
```html
<div class="vision-list">
  <div class="vision-item">
    <div class="v-num {顏色}">①</div>
    <div style="flex:1;">
      <strong ...>要點標題</strong>
      <p ...>說明文字</p>
      <!-- 附經文小塊（選用）-->
      <div class="v-word vw-{顏色}">
        <div class="v-word-label">書卷章節</div>
        經文內容
      </div>
    </div>
  </div>
  <!-- 重複以上 vision-item -->
</div>
```

---

### card-grid — 並列卡片
> 適合：3–4 個平行重點、三信、原則列舉
> 卡片色選 `c-gold / c-dove / c-ruby / c-teal / c-sage`
```html
<div class="card-grid">
  <div class="card c-{顏色}">
    <div class="card-num">一</div>
    <div class="card-icon">emoji</div>
    <h4>卡片標題</h4>
    <p>說明</p>
  </div>
  <!-- 重複 -->
</div>
```

---

### scripture — 聖經原文
> 深色背景金字標題，適合：引用具體章節經文
```html
<div class="scripture">
  <div class="scripture-ref">書卷 章：節</div>
  <p>前段<em>強調語句</em>後段</p>
</div>
```

---

### two-col — 對比雙欄
> 適合：A vs B、屬世 vs 屬靈、正反對比
```html
<div class="two-col">
  <div class="col-side" style="background:var(--ruby-pale);">
    <div class="col-label" style="color:var(--ruby);">標籤</div>
    <div class="col-title">標題</div>
    <ul class="col-list">
      <li style="color:var(--muted);">條列</li>
    </ul>
  </div>
  <div class="col-side" style="background:var(--dove-pale);">
    <!-- 右欄同結構 -->
  </div>
</div>
```

---

### alert — 提醒框
> 適合：牧關提醒、進度預告、補充說明
> 種類：`warn`（黃）、`info`（藍）
```html
<div class="alert warn">
  <span class="alert-icon">📌</span>
  <p><strong>標題：</strong>內容</p>
</div>
```

---

### person-hdr — 發言者標示
> 適合：師母分享、教師回應等有明確發言者的區塊
```html
<div class="person-hdr">
  <div class="person-avatar">姓</div>
  <div>
    <div class="person-name">發言者姓名</div>
    <div class="person-role">引言或職稱</div>
  </div>
</div>
```

---

## 內容整理原則

1. **重點歸納，不是逐字翻寫**——將講道/查經內容提煉成：核心論點 + 支撐經文 + 延伸說明
2. **Section 劃分邏輯**——依主題切，不依時間順序。通常可切成：背景脈絡 → 主要論點 A → 主要論點 B → 延伸應用 → 禱告收尾
3. **顏色分配**——每個 Section 選一個主題色，五色依序輪用，視覺上有節奏感
4. **經文補充**——講道中提到的聖經章節，務必完整引用在 `scripture` 元件中，讓讀者可以對照原文
5. **元件選擇邏輯**：
   - 有 **論點層次**（第一、第二…）→ `vision-list`
   - 有 **平行並列**（三個原則、四個特點）→ `card-grid`
   - 有 **正反對比** → `two-col`
   - 有 **核心引言** → `qblock`
   - 有 **補充提醒** → `alert`
   - 其餘說明 → `hbox`

---

## 輸入格式

提供以下任一種輸入，AI 即可開始製作：

- **逐字稿**（純文字，直接貼上）
- **筆記**（條列或段落皆可）
- **上傳 txt / pdf / html 附件**

並請告知：
- 輯號與日期
- 聚會類型（主日崇拜 / 查經聚會 / 特會）
- 主領姓名
- 整理日期

---

## 輸出

單一 `.html` 檔，所有 CSS、JS 內嵌，可直接瀏覽器開啟或上傳分享。
