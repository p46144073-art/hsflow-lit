# 高速流場文獻導引 — 維護說明

純靜態站，無建置流程、無相依套件。直接用瀏覽器開 `index.html` 即可，
也可原封不動丟到 GitHub Pages、Netlify 或任何靜態主機。

```
index.html      主站：hero、兩張軌道卡、閱讀規則、資料誠實性
track-a.html    子頁：超音速剪切層與凹槽流場
track-b.html    子頁：Taylor–Maccoll 三維錐形流理論解
assets/data.js  ← 唯一需要編輯的檔案
assets/style.css
assets/app.js   渲染與互動，一般不需動
```

## 一、新增一篇文獻

打開 `assets/data.js`，在 `LIBRARY` 陣列末尾（或適當位置）加一個物件：

```js
{
  id: "A-24", track: "A", year: 2021, method: "數值", topic: "凹槽振盪",
  title: "原文標題不要翻譯",
  authors: "Surname, A.B.; Surname, C.D.",
  venue: "J. Fluid Mech. 912, A34",
  doi: "10.1017/jfm.2020.xxxx",       // 純 DOI，沒有就寫 null
  question: "這篇要回答的可證偽問題（不是摘要）",
  contribution: null,                  // 沒讀原文就留 null
  pages: null,                         // 讀完後填 "p. 461, Fig. 16"
  biblioCheck: "verified",             // verified | unverified | conflict
  note: null
},
```

年代導軌、篩選晶片、統計數字、進度計數全部自動更新，**不需要改任何 HTML**。

### 欄位規則

| 欄位 | 規則 |
|---|---|
| `id` | 軌道代號 + 流水號，全站唯一 |
| `year` | 西元年數字。導軌以 1930–2030 線性映射，超出範圍要改 `app.js` 的 `Y0`/`Y1` |
| `method` | `實驗` `數值` `理論` `綜述` `表格` `教科書`，也可自訂新值 |
| `topic` | 自由字串。填新值就會自動長出新的篩選晶片 |
| `contribution` | **沒讀過原文一律填 `null`**，卡片會顯示「待補」。這是本站的設計前提 |
| `biblioCheck` | `verified` = 對過出版社頁面或兩個以上獨立來源；`unverified` = 單一來源或憑印象；`conflict` = 來源記載相衝突，須在 `note` 說明 |

## 二、新增第三條軌道

1. 在 `data.js` 的 `TRACKS` 加一組設定（`hue` 目前支援 `cyan` / `amber`；
   要加第三色，在 `style.css` 補一條 `[data-hue="xxx"]{ --hue:…; --hue-dim:…; }`）。
2. 複製 `track-a.html` 為 `track-c.html`，改三處：`<html data-hue="…">`、
   頁面標題與 `track-head` 文案、以及底部的 `initTrack("C")`。
3. 在 `index.html` 的 `.tracks` 區塊加一張軌道卡（`id="n-C"` / `span-C` / `v-C`）。

## 三、已知待辦

- 所有卡片的 `pages` 欄位皆為空。關鍵主張要對到頁碼必須逐篇讀 PDF。
- `biblioCheck: "unverified"` 者：A-01（Brown &amp; Roshko 卷期頁）、A-16（Lawson &amp; Barakos 卷期頁）、
  A-17（Sun et al. 作者群與卷期頁）、B-04（Busemann 原文出處與年份）。
- `biblioCheck: "conflict"` 者：A-14、B-01、B-03、B-13，衝突內容已寫在各卡片的 `note`。
- 進度以 `localStorage` 儲存；若瀏覽器封鎖，程式會自動退回記憶體儲存（重整即失效，但不會出錯）。

## 四、離線使用

字型透過 Google Fonts CDN 載入。離線時會退回系統字型（PingFang TC / 微軟正黑體 /
Menlo），版面不會壞掉。若要完全離線，把字型檔下載到 `assets/fonts/` 並改用 `@font-face`。
