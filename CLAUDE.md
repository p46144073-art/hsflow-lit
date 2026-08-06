# 專案規則：高速流場文獻導引

## 這是什麼

單一檔案的靜態文獻導引網站，部署在 GitHub Pages。
**整站只有 `index.html` 一個檔**，CSS、JavaScript、文獻資料全部內嵌其中。
不要建立 `assets/` 資料夾、不要拆分檔案、不要引入建置工具或 npm 相依套件。

資料在 `<script>` 區塊頂端的 `LIBRARY` 陣列，標記行是：
`▼▼▼ 文獻資料庫：要新增文獻就改這裡（LIBRARY 陣列） ▼▼▼`

---

## 不可違反的規則

### 1. 絕對不要編造書目資料

這是本專案存在的理由。使用者是研究生，這些書目會進入論文的參考文獻。

- **不確定的卷、期、頁碼、年份，一律不要填。** 寧可留 `null` 或標 `unverified`
- 不要從記憶中「回想」DOI。DOI 必須來自實際查證
- 如果無法查證，就在 `note` 寫明「未查證」，並把 `biblioCheck` 設為 `"unverified"`
- 發現不同來源記載衝突時，設 `biblioCheck: "conflict"`，並在 `note` 寫清楚兩種說法與建議採用哪一版及理由

### 2. 絕對不要編造 `contribution` 或 `pages`

- `contribution` 只有在**實際讀過原文或摘要**時才填。沒有就是 `null`
- `pages` 只有在確認過該主張出現在該頁時才填。沒有就是 `null`
- **不要用二次文獻的轉述、不要用標題推測、不要用「這類論文通常會說」來填**
- 首頁的「關鍵主張附頁碼」百分比是誠實性指標，不准為了讓數字好看而灌水

### 3. 不要幫使用者「潤飾」追問句成摘要

`question` 欄位必須是**可證偽的問題**，不是內容摘要。
壞例子：「這篇探討了壓縮性效應」
好例子：「成長率下降是壓縮性造成的，還是厚度定義造成的？」

### 4. 動 `LIBRARY` 之後一定要驗語法

改完必須驗證 JS 可解析，例如：

```bash
python3 -c "
import re
h=open('index.html',encoding='utf-8').read()
js=re.findall(r'<script>(.*?)</script>', h, re.S)[0]
open('/tmp/x.js','w',encoding='utf-8').write(js)
"
node --check /tmp/x.js
```

沒有 node 時，至少確認：引號全為英文半形、每個物件結尾有 `,`、括號成對。

**中文全形引號「」是最常見的錯誤來源，絕對不要出現在程式碼中。**

---

## 資料格式

```js
{
  id: "A-24",              // 軌道代號 + 流水號，全站唯一
  track: "A",              // "A" 剪切層與凹槽 | "B" Taylor–Maccoll
  year: 2021,              // 西元年數字（導軌範圍 1930–2030）
  method: "數值",          // 實驗 | 數值 | 理論 | 綜述 | 表格 | 教科書
  topic: "凹槽振盪",       // 自由字串，同名會自動合併成同一個篩選晶片
  title: "原文標題，不要翻譯",
  authors: "Surname, A.B.; Surname, C.D.",
  venue: "J. Fluid Mech. 912, A34",
  doi: "10.1017/jfm.2020.xxx",  // 純 DOI，無網址前綴；沒有填 null
  question: "可證偽的追問句",
  contribution: null,      // 未讀原文一律 null
  pages: null,             // 未確認一律 null
  biblioCheck: "unverified",  // verified | unverified | conflict
  note: null               // 給使用者的備註
},
```

---

## 目前的待辦（不要擅自改成 verified）

- `unverified`：A-01（Brown & Roshko 卷期頁）、A-16（Lawson & Barakos 卷期頁）、
  A-17（Sun et al. 作者群與卷期頁）、B-04（Busemann 原文出處與年份，最弱的一筆）
- `conflict`：A-14、B-01、B-03、B-13
- 全部 46 筆的 `pages` 皆為空

要把 `unverified` 升成 `verified`，**必須實際查證出版社頁面並在回覆中說明查證來源**。

---

## 工作方式

- 動 `LIBRARY` 以外的地方（版面、配色、程式邏輯）之前先說明你要改什麼，等確認再動
- 每次 commit 只做一件事，訊息用英文簡述，例如 `add A-24`、`verify biblio for A-01`
- 不要主動 push 到 `main`。除非使用者明講，否則開分支
- 使用者是流體力學研究生，不是前端工程師。解釋時說結果和影響，不要說 CSS 選擇器細節
- 回答用繁體中文、台灣用語

---

## 部署

GitHub Pages，`main` 分支根目錄。push 後約 1–2 分鐘生效。
網址：`https://p46144073-art.github.io/hsflow-lit/`
