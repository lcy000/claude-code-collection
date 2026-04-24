# claude-hud — Status Bar Plugin

> 插件由 [jarrodwatts](https://github.com/jarrodwatts/claude-hud) 開發，以下為個人設定分享

在 Claude Code 終端機下方顯示一條常駐狀態列，即時呈現目前 session 的狀態，不用輸入指令就能掌握：

- 目前使用的模型
- Context 視窗剩餘空間（視覺化進度條）
- Token 用量與費用
- Agent 執行狀態
- Git 分支名稱
- Session 時長、速度等資訊

---

## 安裝步驟

1. 新增市場：`/plugin marketplace add jarrodwatts/claude-hud`
2. 安裝插件：`/plugin install claude-hud`
3. 執行設定：`/claude-hud:setup`（會自動設定 statusLine 並啟用）
4. 重啟 Claude Code
   > 重啟後沒看到 bar 出現，可能是 bug，請 Claude Code 除錯

5. 將以下內容存到 `~/.claude/plugins/claude-hud/config.json`（覆蓋原本的）

---

## 設定檔

```json
{
  "lineLayout": "compact",
  "showSeparators": false,
  "gitStatus": {
    "enabled": true,
    "showDirty": false,
    "showAheadBehind": false,
    "showFileStats": false
  },
  "display": {
    "showModel": true,
    "showContextBar": true,
    "showTools": false,
    "showAgents": true,
    "showTodos": false,
    "showProject": false,
    "showConfigCounts": true,
    "showTokenBreakdown": false,
    "showSpeed": false,
    "showUsage": true,
    "usageBarEnabled": false,
    "showSessionName": false,
    "showDuration": false,
    "customLine": ""
  }
}
```

---

## 目前開啟的功能

| 功能 | 說明 |
|------|------|
| Model | 目前使用的模型名稱 |
| Context bar | Context 視窗使用量（視覺化） |
| Agents | 子 agent 執行狀態 |
| Config counts | 目前載入的設定數量 |
| Usage | Token 用量與費用（文字格式） |
| Git branch | 目前 git 分支名稱 |

---

## 目前關閉的功能

| 功能 | 說明 | config 鍵 |
|------|------|-----------|
| Separators | 各項目之間的分隔線 | `showSeparators` |
| Tools | 本次 session 使用的工具次數 | `showTools` |
| Todos | Todo 項目完成進度 | `showTodos` |
| Project | 專案名稱 | `showProject` |
| Token breakdown | Input / Output / Cache token 各別數量 | `showTokenBreakdown` |
| Speed | 輸出速度（tokens/s） | `showSpeed` |
| Usage bar | 費用用量（視覺化 bar 格式，與 Usage 文字版擇一） | `usageBarEnabled` |
| Session name | Session 名稱 | `showSessionName` |
| Duration | Session 持續時間 | `showDuration` |
| Custom line | 自訂文字列 | `customLine` |
| Git dirty | 未提交的異動檔案數 | `gitStatus.showDirty` |
| Git ahead/behind | 與遠端分支的 commit 差距 | `gitStatus.showAheadBehind` |
| Git file stats | 新增／修改／刪除的檔案統計 | `gitStatus.showFileStats` |

---

## 調整設定的方式

**推薦**：直接對 Claude Code 說要調整哪些項目，會自動修改設定：

```
/claude-hud:configure
```

**手動**：直接編輯 `~/.claude/plugins/claude-hud/config.json`，將對應鍵改為 `true` / `false`
