# Campaign Asset Pipeline

每個 stage 背景圖都應該是無人物、無士兵、無文字的空景，維持現有三國題材的陰鬱數位油畫與電影感光影方向。Prompt 需描述地點、季節／天候、主要地形、勢力色調與構圖用途，但不要生成 UI 文字或角色肖像。

## 交付規則

- 原始 AI 生成物放在 `artifacts/campaign/{NNN}/raw-images/`。
- 最終卡片背景放在 `public/assets/campaign/{NNN}/`。
- 檔名必須是 `stage_{chapter}_{order}.webp`。
- 目標尺寸為 256×256；可使用 repository 的 `optimize_images.js --campaign-webp --quality=82` 轉換與壓縮。
- 每個 stage 都要有 manifest entry；缺圖時保留清單並讓 strict validation 失敗。

若圖片生成工具不可用，仍要完成 Prompt、檔名規劃與缺圖報告，不要以不存在的圖片路徑宣稱完成。
