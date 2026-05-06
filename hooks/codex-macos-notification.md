# Codex macOS Notification Hooks

兩個 hooks 設定，偵測使用者是否正在看著 Codex：
- **權限請求提醒**：有提示音 + 通知
- **完成提醒**：靜音通知

> Codex 的 `PermissionRequest` hook 會在權限 UI 顯示前觸發，所以提示音會比畫面上的權限詢問更早出現。這和 Claude Code 的 `Notification` hook 不同。

---

## 設定位置

Codex hooks 需要兩個檔案：

| 用途 | 路徑 |
|------|------|
| 開啟 hooks 功能 | `~/.codex/config.toml` |
| 放 hooks 設定 | `~/.codex/hooks.json` |

---

## 開啟 hooks 功能

在 `~/.codex/config.toml` 加入：

```toml
[features]
codex_hooks = true
```

如果原本已經有 `[features]` 區塊，把 `codex_hooks = true` 加進既有區塊即可。

---

## Hooks 設定

將以下內容放到 `~/.codex/hooks.json`：

```json
{
  "hooks": {
    "PermissionRequest": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "afplay /System/Library/Sounds/Funk.aiff -v 5; osascript -e 'tell application \"System Events\" to set frontApp to name of first application process whose frontmost is true' -e 'if frontApp is not in {\"Terminal\", \"Code\"} then display notification \"Codex 需要你的注意\" with title \"Codex\"'"
          }
        ]
      }
    ],
    "Stop": [
      {
        "matcher": "",
        "hooks": [
          {
            "type": "command",
            "command": "osascript -e 'tell application \"System Events\" to set frontApp to name of first application process whose frontmost is true' -e 'if frontApp is not in {\"Terminal\", \"Code\"} then display notification \"Codex 已完成，等待回應\" with title \"Codex\"'"
          }
        ]
      }
    ]
  }
}
```

---

## 和 Claude Code 版本的差異

| 項目 | Claude Code | Codex |
|------|-------------|-------|
| 設定檔 | `~/.claude/settings.json` | `~/.codex/config.toml` + `~/.codex/hooks.json` |
| 權限提醒事件 | `Notification` | `PermissionRequest` |
| 權限提醒時機 | 權限提示出現後 | 權限 UI 顯示前 |
| 完成提醒事件 | `Stop` | `Stop` |

---

## 調整提醒音效大小

`afplay` 支援 `-v` 參數設定音量倍數，預設為 `1.0`（跟著系統媒體音量走）：

上方設定檔使用 `-v 5`，實測比較好辨識。可自行調整：

| `-v` 值 | 效果 |
|---------|------|
| 不加 `-v` | 完全依賴系統媒體音量 |
| `1.5` | 放大 1.5 倍 |
| `2.0` | 放大 2 倍 |
| `5` | 明顯大聲，實測比較好辨識（推薦、目前設定） |

修改指令中的數字即可，例如：`afplay /System/Library/Sounds/Funk.aiff -v 2`。

---

## 注意事項

- 只適用於 macOS
- 修改後建議重啟 Codex session
- 通知音效 `Funk` 可換成其他（`Basso`、`Glass`、`Hero` 等）
- 需確認你使用的終端機或編輯器 app 名稱，將 `{"Terminal", "Code"}` 改成對應的名稱
  - VS Code 的 app 名稱是 `Code`
  - iTerm2 的 app 名稱是 `iTerm2`
  - Cursor 的 app 名稱是 `Cursor`
