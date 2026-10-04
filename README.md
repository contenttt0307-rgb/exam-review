# 段考複習系統

高一數學第一次期中考（單元 01–04：實數、絕對值、指數、科學記號與常用對數）的手機刷題網頁 App。

目前是**原型版本**：有 6 題範例題、60 秒健檢、錯因追問、健檢結果和首頁。學校 Google 帳號登入和進度保存還是模擬畫面，要等串接 Firebase 之後才會真的運作。

## 檔案說明

| 檔案 | 用途 |
| --- | --- |
| `index.html` | App 主程式（介面與操作流程） |
| `questions.js` | 題庫。數學式一律用 LaTeX 撰寫，行內公式用 `\( ... \)` 包起來 |
| `manifest.webmanifest` | 讓學生可以把網頁「加到主畫面」，像 App 一樣開啟 |
| `icon-192.png`、`icon-512.png` | App 圖示 |
| `.gitignore` | 防止學號對照表（.xlsx）和課本 PDF 被上傳 |

## 發布到 GitHub Pages

1. 在 GitHub 建立新的儲存庫（Repository），例如命名為 `exam-review`，設為 **Public**。
2. 進入儲存庫，按 **Add file → Upload files**，把這個資料夾裡的所有檔案拖進去，按 **Commit changes**。
3. 到儲存庫的 **Settings → Pages**，在 **Branch** 選 `main`、資料夾選 `/ (root)`，按 **Save**。
4. 等 1～2 分鐘，頁面上方會出現網址，格式是 `https://你的帳號.github.io/exam-review/`。用手機打開就能使用。

> 上傳前請確認：學號對照表和課本 PDF **不在**上傳的檔案裡。這兩種檔案含有學生個資或有著作權。

## 新增或修改題目

打開 `questions.js`，照現有題目的格式新增一筆。每題的欄位：

- `unit`、`unitName`：單元編號與名稱
- `tag`：觀念代碼與名稱（對應規劃文件的觀念清單，例如 `A04`）
- `type`：`tf` 是非、`mc` 單選、`multi` 多選、`num` 數字填充
- `stem`：題幹；`opts`：選項；`ans`：正解（選項的編號，從 0 開始；`num` 題直接填數字字串）
- `reasons`：答錯時追問的 3 個錯因；`key`：其中最常見的那一個（編號）
- `exp`：解析；`fast`：補充快解（可省略）
- `stuck`：全班卡關比例。目前是範例數字，串接資料庫後會改成真實統計

LaTeX 寫在 `String.raw` 字串裡，反斜線只要打一次，例如 ``String.raw`\(\sqrt{5}-2\)` ``。

## 下一階段：串接 Firebase

1. 用老師的 Google 帳號到 Firebase 建立專案，開啟 **Authentication → Google 登入**，並限定只接受 `sssh.tp.edu.tw` 網域的帳號。
2. 開啟 **Firestore 資料庫**，設定存取規則：學生只能讀寫自己的進度，老師帳號可以讀取全班資料。
3. 把學號對照表匯入 Firestore（只有老師帳號能讀），**不要**放進 GitHub。
4. 請學校資訊組在 Google Workspace 管理控制台，把這個 Firebase 專案的登入程式設為「信任」。未滿 18 歲的學生帳號預設會擋下未設定的第三方 App。
5. 在 Firebase 的 **Authentication → Settings → Authorized domains** 加入 `你的帳號.github.io`。
