# 剛好學（最新版 V5）｜Firebase 即時互動教室（單檔 HTML）

一個「老師開教室 → 學生用代碼加入 → 老師一鍵切換互動模式 → 即時收作答/統計/互評/下載」的 Web 互動教室工具。  
本專案為**單一 HTML 檔**即可運作（可搭配 Firebase 做多人即時同步）。

---

## 目錄

- [特色與功能總覽](#特色與功能總覽)
- [快速開始](#快速開始)
- [使用流程（老師 / 學生）](#使用流程老師--學生)
- [互動模式清單](#互動模式清單)
- [教師監看/管理功能](#教師監看管理功能)
- [資料結構概念](#資料結構概念)
- [Firebase 設定](#firebase-設定)
  - [建立 Firebase 專案](#建立-firebase-專案)
  - [需要開啟的 Firebase 服務](#需要開啟的-firebase-服務)
  - [Firebase 設定值（firebaseConfig）要填哪些欄位](#firebase-設定值firebaseconfig要填哪些欄位)
  - [firebaseConfig 填入方式（全部方法）](#firebaseconfig-填入方式全部方法)
    - [方法 1：直接改 HTML 內建 hardcodedConfig（最高優先）](#方法-1直接改-html-內建-hardcodedconfig最高優先)
    - [方法 2：用 URL 參數 config= 帶入（可生成 QRCode/短網址）](#方法-2用-url-參數-config-帶入可生成-qrcode短網址)
    - [方法 3：用「Firebase 設定」視窗貼上並存到 localStorage](#方法-3用-firebase-設定-視窗貼上並存到-localstorage)
    - [方法 4：外掛 firebase-config.js（最低優先）](#方法-4外掛-firebase-configjs最低優先)
    - [方法 5：在同一台裝置多教室切換建議](#方法-5在同一台裝置多教室切換建議)
- [部署到 GitHub Pages](#部署到-github-pages)
- [常見問題](#常見問題)
- [授權](#授權)

---

## 特色與功能總覽

- ✅ **老師/學生雙角色**：老師建立（自訂）教室代碼；學生輸入代碼加入。
- ✅ **即時互動**：老師切換互動模式後，學生端即時切換畫面。
- ✅ **多互動模式**：是非/選擇/文字/繪圖/錄音/文字雲/便條紙牆/快速投票/搶答/閱讀測驗/排序/配對/專注遊戲/派送網址或 HTML。
- ✅ **教師監看面板**：暫停作答、抽籤、未作答名單、AI 分析、下載作答/媒體、統計圖表、互評、查看題目…等。
- ✅ **題庫/設定**：選擇題、排序、配對、閱讀測驗皆支援題庫與設定流程。
- ✅ **可選 AI**：內建 AI 設定與出題/分析流程（需自行填 API Key）。
- ✅ **單檔 HTML**：開箱即用、好部署、好備份。

---


## 使用流程（老師 / 學生）

### 老師
1. 打開網頁 → 選「教師」
2.（可能需要密碼驗證；見 [常見問題](#常見問題)）
3. 輸入/選擇教室代碼 → 進入教師面板
4. 在「互動模式」選單選一個模式（或先進入設定視窗）
5. 切到「監看」即可即時看學生作答，並使用工具列功能（暫停、抽籤、統計、下載等）

### 學生
1. 打開網頁 → 選「學生」
2. 輸入「教室代碼」＋「姓名」
3. 等待老師開始互動（老師切模式後畫面會自動切換）
4. 依模式作答並送出

---

## 互動模式清單

> 以下是老師在「互動模式」可選的模式（同一教室同一時間一種模式）。

- `waiting`：等待畫面（老師尚未開始）
- `true_false`：是非題（學生點選 T/F）
- `multiple_choice`：選擇題（可派送自訂題目/題庫）
- `text_input`：文字輸入作答
- `drawing`：繪圖作答（學生畫板；老師可點擊放大檢視）
- `recording`：錄音作答（學生錄音上傳；老師可下載媒體）
- `word_cloud`：文字雲（學生輸入詞彙；老師端即時雲圖顯示）
- `sticky_note`：便條紙牆（學生貼便條；老師端牆面可縮放/檢視）
- `quick_poll`：快速投票（即時統計）
- `quick_answer`：搶答（先搶到者顯示勝利）
- `sequencing`：排序題（拖拉排序）
- `matching`：配對題（配對連線/選擇）
- `reading_comprehension`：閱讀測驗（文章＋題組）
- `focus_game`：專注遊戲（方格遊戲/設定格數等）
- `url_dispatch`：派送網址/HTML（老師派送連結、YouTube 或自訂 HTML）

---

## 教師監看/管理功能

老師在「監看」面板可用的常用功能：

- **暫停 / 恢復**：一鍵暫停學生作答（學生端會顯示暫停遮罩）
- **抽籤**：可從「已作答」或「在線名單」抽出學生
- **未作答名單**：列出尚未提交的學生
- **AI 分析**：對作答做 AI 摘要/分析（需先設定 AI Key）
- **下載媒體**：錄音/媒體類作答可打包下載
- **統計圖表**：依模式顯示統計（例如投票/選擇題）
- **互評**：開啟互評流程（學生端會進入互評）
- **題目檢視**：查看目前派送題目（含閱讀題組）
- **下載作答**：匯出學生作答資料
- **下課/結束互動**：清空教室狀態，避免學生看到上一堂課的模式/資料

---

## 資料結構概念

本工具使用 Firestore 以「教室代碼」做分流概念（每個教室是一個子樹）。  
常見概念：
- `settings/control`：老師控制（目前互動模式、是否暫停、派送設定、題組設定…）
- `presence`：在線名單
- `studentResponses`：學生作答集合（依模式內容不同）

> 你可以依校內需求自行調整 collection/doc 命名或安全規則。

---

## Firebase 設定

### 建立 Firebase 專案
1. 前往 Firebase Console → 建立專案
2. 新增 Web App（</>）→ 取得 `firebaseConfig`

### 需要開啟的 Firebase 服務
- ✅ **Firestore Database**（必需）：即時同步教室狀態、作答、在線名單
- ✅ **Authentication**（建議）：本工具通常會使用匿名登入/等效登入流程以便寫入資料

### Firebase 設定值（firebaseConfig）要填哪些欄位
典型 Web 設定如下（依你的 Firebase 專案為準）：

```js
{
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID",
}
```

---

## firebaseConfig 填入方式（全部方法）

> **載入優先順序（由高到低）**  
> 1) HTML 內建 `hardcodedConfig` → 2) URL 參數 `?config=` → 3) localStorage → 4) 外掛 `firebase-config.js`

---

### 方法 1：直接改 HTML 內建 hardcodedConfig（最高優先）

適合：**只部署一份固定專案、最穩定**

1. 打開 `剛好學最新版V5.html`
2. 找到 `hardcodedConfig`（或類似命名區塊）
3. 把你的 `firebaseConfig` 貼上並保存
4. 重新整理

✅ 優點：最穩定、最少步驟  
⚠️ 注意：此方式會覆蓋其他來源（URL/localStorage/firebase-config.js）

---

### 方法 2：用 URL 參數 config= 帶入（可生成 QRCode/短網址）

適合：**同一份 HTML 需要給不同人/不同 Firebase 專案使用**

此程式支援從網址讀取：
- `?config=`：通常為「Base64 編碼後的 firebaseConfig JSON」



---

### 方法 3：用「Firebase 設定」視窗貼上並存到 localStorage

適合：**在同一台教室電腦上長期使用**

1. 打開網頁
2. 進入「Firebase 設定」
3. 把 `firebaseConfig` 貼到文字框
4. 按「套用/儲存」  
   → 會存到瀏覽器 localStorage，下次開啟仍有效

> 貼上時格式可較寬鬆（程式會嘗試自動補引號/轉 JSON），但仍建議使用標準 JSON。

✅ 優點：不用改檔、適合固定教室電腦  
⚠️ 注意：清除瀏覽器資料會失效

#### 3-1. 直接用內建功能「生成專用網址 / QRCode」
在頁面內的「Firebase 設定」視窗通常會有：
- 生成「專用網址」
- 生成「短網址」
- 生成「QRCode」

> 使用方法2時，配合這個方式是最推薦的方式，因為不用你手動 Base64。


✅ 優點：分享方便、可做 QRCode  
⚠️ 注意：`config` 可能會被寫入 localStorage（同台裝置後續可直接使用）

---

### 方法 4：外掛 firebase-config.js（最低優先）

適合：**你想把設定跟主 HTML 拆開、或用不同環境注入**

在同一層放一個 `firebase-config.js`，內容例：

```js
// firebase-config.js
// 方式：定義全域變數 __firebase_config（字串型 JSON）
window.__firebase_config = JSON.stringify({
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
});
```

然後在 HTML 中（通常已經內建）確保有載入：
```html
<script src="./firebase-config.js"></script>
```

✅ 優點：設定檔可分離  
⚠️ 注意：若同時存在 hardcodedConfig / URL / localStorage，這個會被覆蓋

---

### 方法 5：在同一台裝置多教室切換建議

- 你若常切換不同 Firebase 專案，建議用：
  - **URL `?config=` 分享方式**（每次開不同連結）
  - 或在「Firebase 設定」視窗中先「清除 localStorage」再貼新設定

---

## 部署到 GitHub Pages

1. 建立 repo（例如 `just-right-classroom`）
2. 把 `剛好學最新版V5.html` 放到 repo 根目錄（或 `/docs`）
3. GitHub → Settings → Pages  
   - Source：`main` branch  
   - Folder：`/root` 或 `/docs`
4. 取得 Pages 網址後即可使用

> 若你使用「方法 3：URL ?config=」，部署到 Pages 後會更方便分享給學生掃 QRCode 直接進入。

---

## 常見問題

### Q1：為什麼我用檔案總管雙擊開啟，某些功能怪怪的？
A：請用 http(s) 方式開啟（Live Server / `python -m http.server`），避免瀏覽器對 `file://` 的限制。

### Q2：老師進入需要密碼，可以關掉嗎？
A：程式支援在網址帶 `?passwd=no` 跳過教師密碼視窗，例如：
```
https://你的網域/剛好學最新版V5.html?passwd=no
```

### Q3：學生說看到上一堂課的互動模式？
A：老師「進入教室」或「再進入」時會把模式重置為 `waiting`，也建議老師下課前按「下課/結束」清空狀態。

### Q4：AI 功能要怎麼用？
A：到「AI 設定」填入你的 API Key（例如 Gemini），之後在題目設定或監看面板按「AI 出題/AI 分析」即可。

---

## 授權

你可依需求自行加上 License（MIT / Apache-2.0 / 校內專用等）。
