# 高速流場文獻導引

**單一檔案。** 整個網站就是 `index.html` 一個檔，CSS、JavaScript、文獻資料全部包在裡面。
沒有 assets 資料夾、沒有相對路徑、沒有建置流程。

## 使用

- **本機**：雙擊 `index.html`
- **GitHub Pages**：把 `index.html` 上傳到 repository 根目錄即可

## 新增文獻

用文字編輯器（VS Code / Notepad++）打開 `index.html`，搜尋 `LIBRARY`，
會找到這一行標記：

```
▼▼▼ 文獻資料庫：要新增文獻就改這裡（LIBRARY 陣列） ▼▼▼
```

在陣列裡加一個物件即可：

```js
{
  id: "A-24", track: "A", year: 2021, method: "數值", topic: "凹槽振盪",
  title: "原文標題不要翻譯",
  authors: "Surname, A.B.; Surname, C.D.",
  venue: "J. Fluid Mech. 912, A34",
  doi: "10.1017/jfm.2020.xxxx",   // 沒有就填 null
  question: "這篇要回答的可證偽問題（不是摘要）",
  contribution: null,              // 沒讀原文一律 null
  pages: null,                     // 讀完填 "p. 461, Fig. 16"
  biblioCheck: "verified",         // verified | unverified | conflict
  note: null
},
```

年代導軌、側欄編號、篩選晶片、統計數字、進度計數全部自動更新。

### 常見錯誤

| 症狀 | 原因 |
|---|---|
| 頁面空白 | `LIBRARY` 裡漏逗號，或用了中文全形引號「」。必須是英文半形 `"` |
| 新 topic 沒出現晶片 | 字串打錯字，要合併必須逐字相同 |
| 刻度跑到軸外 | `year` 超出 1930–2030，需改程式裡的 `Y0` / `Y1` |

出錯時按 F12 看 Console，錯誤行號會直接指出來。

## 待辦

- 所有 `pages` 欄位皆空，需逐篇讀 PDF 回填
- `unverified` 四筆：A-01、A-16、A-17、B-04（B-04 Busemann 原文出處最弱）
- `conflict` 四筆：A-14、B-01、B-03、B-13，衝突內容已寫在各卡片 note

## 其他

進度以 localStorage 儲存，被瀏覽器封鎖時自動退回記憶體（重整失效但不會出錯）。
字型走 Google Fonts CDN，離線時退回系統字型，版面不會壞。
