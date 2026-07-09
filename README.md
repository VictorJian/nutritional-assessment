# nutritional-assessment

營養評估表前端。這是一個靜態 HTML 頁面，使用者填寫姓名、年齡、填表日期並勾選評估項目後，可以自動計算建議補充項目，並將資料送到後端 API 儲存。

## 功能

- 姓名與年齡必填。
- 勾選左側「是」後自動計算各營養補充品分數。
- 送出資料到後端後，後端會回傳一組 `YYYYMMDDHHMMSS` 格式的紀錄 ID。
- 送出或查詢成功後，表格會套回當時勾選「是」的項目並固定為不可編輯。
- 可用紀錄 ID 查詢單筆資料。
- 可用姓名查詢多筆資料，並從符合筆數中選擇一筆查看。
- 可複製分享網址；網址會帶上 `?id=<紀錄ID>`，開啟後會自動查詢該筆資料。
- 頁面載入時會背景呼叫後端 `/ping`，讓 Render 免費服務先醒來。
- 可列印。
- 可直接下載 PDF 檔。
- 可按「再寫一張」清空畫面並重新填寫。

## 檔案

- `index.html`：主要前端頁面與所有互動邏輯。
- `favicon.png`：網頁頁籤小圖示。
- `source-assessment.jpg`：原始評估表參考圖。

## 使用方式

先啟動後端 API，預設位置是：

```sh
http://localhost:8080
```

接著直接用瀏覽器開啟 `index.html`。

前端目前預設呼叫 Render 後端：

```text
https://nutritional-assessment-api.onrender.com/api/assessments
```

如果後端不是跑在預設位置，前端 API 位置需要同步調整：

```js
window.NUTRITION_API_BASE_URL = "http://localhost:8080";
```

## 操作流程

1. 填寫姓名、年齡、填表日期。
2. 勾選符合的評估項目。
3. 確認總計與建議補充。
4. 按「送出資料」儲存到後端。
5. 送出後畫面會固定，不能再編輯。
6. 需要重新填寫時，按上方「再寫一張」。

## 查詢與分享

送出成功後會顯示紀錄 ID。使用者可以：

- 在「紀錄 ID」輸入 ID 查詢。
- 在「姓名」輸入姓名查詢。
- 按「分享網址」複製目前紀錄網址。

分享網址格式：

```text
index.html?id=20260708110511
```

開啟分享網址時，前端會用該 ID 向後端取得資料並顯示結果。

## PDF

按「存成 PDF」會直接產生並下載 PDF 檔，不會開啟列印視窗。PDF 檔名會包含：

```text
營養評估表_姓名_日期_紀錄ID.pdf
```

## 後端需求

此前端需搭配 `nutritional-assessment-api` 使用。後端需開啟 CORS，並支援：

- `POST /api/assessments`
- `GET /api/assessments/:id`
- `GET /api/assessments?name=<name>`
- `GET /ping`
