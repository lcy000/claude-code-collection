# macOS Notification Hooks

兩個 hooks 設定，偵測使用者是否正在看著 Claude Code：
- **權限請求提醒**：有提示音 + 通知
- **完成提醒**：靜音通知

---

## 設定位置

將 `"hooks"` 區塊貼入以下任一設定檔：

| 範圍 | 路徑 |
|------|------|
| 全域（所有專案） | `~/.claude/settings.json` |
| 單一專案 | 專案根目錄下的 `.claude/settings.json` |

在既有的 JSON 物件中加入 `"hooks"` 鍵即可，例如：

```json
{
  "hooks": {
    ...貼入下方內容...
  }
}
```

---

## Hooks 設定

```json
"hooks": {
  "Stop": [
    {
      "matcher": "",
      "hooks": [
        {
          "type": "command",
          "command": "osascript -e 'tell application \"System Events\" to set frontApp to name of first application process whose frontmost is true' -e 'if frontApp is not in {\"Terminal\", \"Code\"} then display notification \"Claude 已完成，等待回應\" with title \"Claude Code\"'"
        }
      ]
    }
  ],
  "Notification": [
    {
      "matcher": "",
      "hooks": [
        {
          "type": "command",
          "command": "input=$(cat); if echo \"$input\" | grep -q '\"notification_type\":\"permission_prompt\"'; then afplay /System/Library/Sounds/Funk.aiff; osascript -e 'tell application \"System Events\" to set frontApp to name of first application process whose frontmost is true' -e 'if frontApp is not in {\"Terminal\", \"Code\"} then display notification \"Claude 需要你的注意\" with title \"Claude Code\"'; fi"
        }
      ]
    }
  ]
}
```

---

## 注意事項

- 只適用於 macOS
- 通知音效 `Funk` 可換成其他（`Basso`、`Glass`、`Hero` 等）
- 需確認你使用的終端機或編輯器 app 名稱，將 `{"Terminal", "Code"}` 改成對應的名稱
  - VS Code 的 app 名稱是 `Code`
  - iTerm2 的 app 名稱是 `iTerm2`
  - Cursor 的 app 名稱是 `Cursor`
