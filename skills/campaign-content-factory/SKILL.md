---
name: campaign-content-factory
description: "分析現有三國卡牌遊戲的 campaign 進度，依前章自動推導、生成與驗證下一個戰役章節及其圖片資產。"
---

# Campaign Content Factory

當使用者要求「新增下一章」、「接續目前戰役生成關卡」、「補完戰役章節」或檢查戰役內容時使用本 Skill。

## 核心原則

- 預設採低輸入模式：不要要求使用者先填完整章節規格；先從 repository 與既有 campaign 自動盤點，再提出推導結果。
- AI 負責劇情、敵人組合、難度曲線、獎勵草案與圖片 Prompt；固定驗證器負責 ID、結構、資產與可執行性。
- 不把暫存生成物直接當成正式遊戲資料。先寫入 `artifacts/campaign/{NNN}/`，通過檢查並獲得使用者確認後，才更新正式資料與資產。
- 維持既有 stage ID（例如 `stage_7_1`）與資料欄位慣例；不要為了生成方便更改戰鬥引擎介面。
- 若現有章節有缺圖、無效引用或其他歷史問題，列為報告項目，不要偷偷改動未被要求的舊章節。

## 工作流程

1. 確認工作目錄是遊戲 repository，閱讀 [repository-map.md](references/repository-map.md)；需要判斷下一章節進度時再閱讀 [progression-policy.md](references/progression-policy.md)。
2. 執行 `node tools/campaign/inspect_campaign.mjs --json`，以輸出內容作為現況基線，不依賴記憶猜測最後章節。
3. 從基線自動推導下一章編號、關卡數、難度梯度、劇情承接、敵人重複風險、獎勵方向與資產需求。只有遇到真正無法判斷的產品決策才詢問使用者；可選參數（主題、關卡數、是否只用既有武將）不能變成必要表單。
4. 閱讀 [campaign-schema.md](references/campaign-schema.md)，生成章節 metadata、stage data、Prompt 與 `inferred-plan.json`。先放在 `artifacts/campaign/{NNN}/`，不要直接覆蓋正式檔。
5. 閱讀 [asset-pipeline.md](references/asset-pipeline.md)。需要 raster 圖片時使用可用的 AI 圖像生成能力，逐關卡輸出並確認檔名與尺寸；沒有圖像生成能力時，保留 Prompt 並清楚標記未完成資產。
6. 執行 `node tools/campaign/validate_campaign.mjs --json`；若要宣稱章節可發布，必須再執行 `--strict-assets`，並確認沒有 error 或該章節相關 warning。
7. 生成 validation/balance/review 報告。只有使用者要求正式匯入或明確接受生成結果時，才以 patch 更新 `src/data/campaign/chapters/{NNN}/`、chapters registry、asset manifest 與 `public/assets/campaign/{NNN}/`。

## 輸出標準

分析模式應回報現況、推導依據、下一章草案與風險。生成模式應至少產生：章節 metadata、每關 stage 設定、每關背景 Prompt、圖片資產或缺圖清單、驗證報告，以及需要人工決定的事項。所有劇情與報告以繁體中文為主。

不要把 `overrides`、`extraStatuses`、獎勵或 AI difficulty 當成純文字描述；它們必須落在可被現有 runtime 讀取的資料欄位，並由 validator 檢查。

詳細資料契約、難度推導與圖片規則只在對應工作需要時讀取：

- [campaign-schema.md](references/campaign-schema.md)：資料檔案與欄位。
- [progression-policy.md](references/progression-policy.md)：從舊章節推導下一章的方法。
- [asset-pipeline.md](references/asset-pipeline.md)：圖片、Prompt 與 manifest。
