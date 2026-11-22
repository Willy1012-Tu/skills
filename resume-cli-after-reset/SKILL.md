---
name: "resume-cli-after-reset"
description: "為 Claude Code CLI 產生額度重置後自動接續工作的 Windows 排程腳本。使用者說額度用完要去睡覺、想讓 CLI 重置後自動繼續時使用。"
---

# 額度重置後自動接續 CLI

目標：使用者的 Claude Code CLI 因額度用完而中斷，讓它在額度重置後，自動從上次的對話接著做。

## 先問清楚（缺什麼問什麼，已知的不重問）

1. 專案資料夾的完整路徑。
2. 帳號設定資料夾。使用第二個帳號時，通常是 CLAUDE_CONFIG_DIR 指向的資料夾；只有一個帳號可省略。
3. 額度重置時間（台北時間）。排程要設在重置之後，並多留幾分鐘。
4. 要用的模型與 effort。只加使用者明說、確定支援的參數，不自己猜。
5. 接續時送出的提示文字。沒指定就用：「額度已重置，請從上次中斷的地方繼續，完成後簡短回報做了什麼、還剩什麼。」

## 為什麼要在使用者自己的電腦上跑

雲端排程任務和 Claude 工作環境的 Shell 通常是 Linux，無法在使用者的 Windows 上執行 claude 指令與切換帳號資料夾，建了也會悄悄失敗。所以不要用雲端排程代替，改為產出 PowerShell 腳本與排程指令，請使用者自己貼上執行。

## 產出內容

步驟 1：建立腳本。

```powershell
@'
Set-Location <專案路徑>
$env:CLAUDE_CONFIG_DIR = "<帳號設定資料夾>"   # 只有一個帳號就刪掉這行
claude --continue <使用者指定的模型與 effort 參數> -p "<提示文字>" *> "$HOME\resume_cli.log"
'@ | Set-Content -Path "$HOME\resume_cli.ps1" -Encoding UTF8
```

步驟 2：建立排程（時間填重置之後）。

```powershell
$a = New-ScheduledTaskAction -Execute "powershell.exe" -Argument "-NoProfile -ExecutionPolicy Bypass -File `"$HOME\resume_cli.ps1`""
$t = New-ScheduledTaskTrigger -Once -At "<YYYY-MM-DD HH:MM>"
$s = New-ScheduledTaskSettingsSet -StartWhenAvailable
Register-ScheduledTask -TaskName "ResumeCLI" -Action $a -Trigger $t -Settings $s
```

`--continue` 會接該資料夾最近的一個對話。想更精準，可改用 `--resume <對話編號>`，編號會在關閉 CLI 時顯示。

## 每次都要提醒使用者

- 要開一個新的 PowerShell 貼上，不要貼進正在跑的 CLI，否則會被當成對話內容送出。
- 電腦要保持開機、登入，不休眠。
- 無互動模式遇到需要權限確認的步驟可能會停住。
- 7 天額度如果也快滿，執行到一半可能又碰到上限。
- 確認排程：`Get-ScheduledTask -TaskName ResumeCLI`；確認腳本在：`Test-Path "$HOME\resume_cli.ps1"`。
- 結果看 `$HOME\resume_cli.log`。
- 取消：`Unregister-ScheduledTask -TaskName ResumeCLI -Confirm:$false`。
- 說明這些指令沒有在使用者的電腦上測試過，第一次請留意結果。