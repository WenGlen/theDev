# theDev Backend

Node.js + Express 後端，以 Google Sheet 為資料庫。

## 環境變數

複製 `.env.example` 為 `.env` 並填入。

**Google 憑證（二擇一，建議用金鑰檔案）：**

- **方式一（建議）**：用金鑰 JSON 檔案，避免私鑰在 .env 換行/跳脫錯誤。  
  在 `.env` 設定 `GOOGLE_APPLICATION_CREDENTIALS` 或 `GOOGLE_KEY_FILE`，值為 JSON 檔的**絕對路徑**，例如：  
  `GOOGLE_APPLICATION_CREDENTIALS=/Users/xxx/theDev-backend/google-service-account.json`  
  請把從 Google Cloud 下載的服務帳號 JSON 放到專案目錄（勿提交 Git），並在 .env 填上該路徑。
- **方式二**：在 `.env` 填 `GOOGLE_CLIENT_EMAIL`、`GOOGLE_PRIVATE_KEY`、`GOOGLE_PROJECT_ID`。私鑰須為單行，換行處用 `\n`（反斜線+n），整段用雙引號包住。

其餘：

- `SHEET_ID`：本專案使用的 Google Sheet ID（同一份試算表內可有多個工作表，如 Course、Booking、theDev）
- `PORT`：伺服器埠號（預設 3000）

## 開發

```bash
npm run dev   # nodemon 監聽
npm start     # 正式啟動
```

## API

- `GET /api/courses`：讀取課程（Course 分頁）
- `POST /api/booking`：寫入報名（Booking 分頁）
- **遊戲測試回饋（theDev 分頁）**
  - `POST /api/feedback/mock`：產生一筆模擬回饋並寫入「theDev」分頁
  - `POST /api/feedback`：提交一筆回饋（body: `回報類型`、`回報區塊`、`回報內容`、`開發版本號`，皆字串；回報時間由後端產生）
  - `GET /api/feedback`：讀取 theDev 分頁回饋列表
- `GET /api/health`：健康檢查

試算表內需有「theDev」分頁；若為空白，首次寫入時會自動建立標題列：回報時間、回報類型、回報區塊、回報內容、開發版本號。

## 上傳到 GitHub（theDev 專案）

本後端要推送到 **theDev** 專案（不是 theDev-backend repo）。在 `theDev-backend` 目錄執行：

```bash
git init
git remote add origin https://github.com/<你的帳號>/theDev.git
git branch -M main
git add .
git commit -m "Initial commit: backend with Google Sheet"
git push -u origin main
```

部署到 Zeabur 時，在後台 Variables 設定上述環境變數，勿將 `.env` 提交至 Git。
