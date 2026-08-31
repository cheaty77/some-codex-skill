# Campaign Repository Map

本 Skill 針對 `three-kingdom-card-game` repository。若實際工作目錄不同，先確認它仍使用相同的 campaign contract，再採用最小相容調整。

## 正式資料

- `src/data/campaign/index.js`：對 runtime 提供 `CAMPAIGN_DB`、章節查詢與資產 manifest export。
- `src/data/campaign/chapters/index.js`：唯一章節 registry。
- `src/data/campaign/chapters/{NNN}/meta.js`：章節 ID、編號與顯示名稱。
- `src/data/campaign/chapters/{NNN}/stages.js`：該章的 stage 陣列；stage ID 仍採 `stage_{chapter}_{order}`。
- `src/services/CampaignService.js`：CPU 隊伍、overrides、extraStatuses 與獎勵的 runtime 邊界。

## 圖片與 Prompt

- `public/assets/campaign/{NNN}/stage_{chapter}_{order}.webp`：卡片背景圖，目標為 256×256 WebP。
- `public/assets/campaign-prompts/{NNN}/backgrounds.md`：章節背景 Prompt 集合；新增章節可另放逐關卡 `.md`。
- `src/data/campaign/assetManifest.js`：stage ID 到 public URL 的唯一 runtime 映射。

## 產出與工具

- `artifacts/campaign/{NNN}/`：AI 推導、草稿、Prompt、原始圖片與驗證報告，不是 runtime source of truth。
- `tools/campaign/inspect_campaign.mjs`：讀取既有內容並輸出可供 AI 使用的現況摘要。
- `tools/campaign/validate_campaign.mjs`：檢查章節、stage、武將、道具、計謀、狀態與資產引用。

新增章節時，正式資料、manifest 與資產檔名必須同步；不要在 Vue 元件內新增章節名稱或圖片路徑特例。
