# Campaign Data Contract

## 章節檔案

```js
export const CHAPTER_7_META = {
    id: "chapter_007",
    chapter: 7,
    name: "章節名稱",
};
```

`src/data/campaign/chapters/{NNN}/stages.js` 匯出 `CHAPTER_{N}_STAGES` 陣列。每個 stage 至少包含：

```js
{
    id: "stage_7_1",
    chapter: 7,
    order: 61,
    name: "關卡名稱",
    description: "繁體中文關卡描述",
    cpuTeam: [{ generalId: "existing_id", stars: 0 }],
    aiDifficulty: "normal",
    cpuStratagemId: null,
    cpuItems: {},
    overrides: {},
    rewards: {
        firstClear: { gold: 0, diamonds: 0, shards: [], items: [], stratagems: [] },
        repeat: { gold: 0 },
    },
}
```

`order` 是整個 campaign 的全域順序，必須接續前一關；`cpuTeam` 內的 `generalId`、物品與計謀 ID 必須存在。

## 覆寫與狀態

`overrides` 可使用絕對值 `hp`／`atk`／`def`、倍率 `hpMultiplier`／`atkMultiplier`／`defMultiplier`、加值 `extraHp`／`extraAtk`／`extraDef`，以及：

```js
extraStatuses: [{ type: "WarDrum", value: 1, duration: 2 }]
```

`extraStatuses` 必須使用 `src/data/statuses.js` 已存在的 status type；它會在 Battle 建立後、`start()` 前套用。

## Registry 與 manifest

新增章節必須在 `src/data/campaign/chapters/index.js` 加入 metadata/stages entry，並在 `src/data/campaign/assetManifest.js` 為每個有圖片的 stage 加入 public URL。不要把缺圖 stage 假裝成已完成；validator 應保留 warning，release 檢查使用 `--strict-assets`。
