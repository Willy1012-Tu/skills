# skills

我自己在用的 Claude skills。每個資料夾是一個獨立的 skill，裡面的 `SKILL.md` 就是全部內容，沒有額外的腳本或相依檔案。

## 目前有哪些 skill

| Skill | 一句話說明 | 什麼時候會被用到 |
|---|---|---|
| [minimal-code](minimal-code/SKILL.md) | 寫程式時只寫解決問題所需的最少程式 | 要求寫程式、改程式、寫腳本時 |
| [plan-check-fix-rules](plan-check-fix-rules/SKILL.md) | 依「規劃 → 檢查 → 修正」的順序工作，再交出答案 | 多步驟推理、分析、寫作、程式、研究類任務，或你說「套用思考守則」時 |
| [resume-cli-after-reset](resume-cli-after-reset/SKILL.md) | 產生額度重置後自動接續 Claude Code CLI 的 Windows 排程腳本 | 額度用完要去睡覺、想讓 CLI 重置後自動繼續時 |
| [github-publish-check](github-publish-check/SKILL.md) | 推送到 GitHub 前掃描密鑰與個人資訊，確認後才推送 | 要把程式或文件推上 GitHub、擔心洩露金鑰時 |
| [study-tutor](study-tutor/SKILL.md) | 依你提供的教材出題、逐題講解錯誤、追蹤弱點章節 | 考試複習、練習題、想知道哪裡不熟時 |
| [project-progress-log](project-progress-log/SKILL.md) | 把專案進度整理成固定格式的紀錄 | 記錄今天進度、整理專題進度、寫週報時 |

## minimal-code

目標是「剛好解決你說的問題，不多不少」。最少不等於不正確：程式仍然要能跑、結果要對。

- 只做被要求的事，不預留「以後可能用到」的參數、設定檔或擴充點。
- 能用一個函式解決就不寫類別，能放在一個檔案就不拆成多個。
- 優先使用語言內建功能和標準函式庫，非必要不引入新套件。
- 註解只寫「為什麼這樣做」且不明顯的地方。
- 錯誤處理只放在會合理出錯的邊界（使用者輸入、檔案、網路）。
- 修改既有程式時，只改需要改的地方，不順手重構。
- 交出之前一定實際執行過；你明確要求加註解、測試或拆檔時，以你的要求為準。

## plan-check-fix-rules

處理任務時依序套用三組規則。簡單的一句話問答直接回答，不需要走完整流程。

1. **規劃（動手之前）**：先寫出要交付什麼、怎樣算完成；把資訊分成「已知」「假設」「必須查證」；評估猜錯的代價；先做最不確定的那一步。
2. **檢查（交出之前）**：逐項對照原始要求；數字重算、程式實際執行；對關鍵結論問「如果這是錯的，最可能錯在哪」；標明信心程度。
3. **修正（發現問題之後）**：先找根本原因；修完重跑全部檢查；同一個方法失敗兩次就換方法；做不到就明說。

## resume-cli-after-reset

Claude Code CLI 因額度用完中斷時，產生一份 PowerShell 腳本和 Windows 工作排程器指令，讓它在額度重置後自動從上次的對話接續。

- 會先問：專案路徑、帳號設定資料夾、重置時間、要用的模型與 effort、接續時的提示文字。
- 一定在你自己的電腦上執行，不使用雲端排程（雲端環境無法操作你的 Windows）。
- 每次都會提醒：要貼到新開的 PowerShell、電腦要保持開機登入、無互動模式可能卡在權限確認、如何確認與取消排程。
- 注意：這組指令沒有在每個環境測試過，第一次使用請留意結果。

## github-publish-check

推上公開倉庫的內容等於公開，這個 skill 讓每次推送都走同一套流程：

1. 確認要推到哪個倉庫、是公開還是私人，並列出這次會提交的檔案。
2. 掃描密鑰（api key、token、private key、`.env` 等）並確認 `.gitignore`。
3. 掃描個人資訊（本機路徑、真實姓名、Email、提交作者資訊）。
4. 檢查 git 歷史：密鑰只要進過提交，刪檔也還在歷史裡，必須先作廢並重新產生金鑰。
5. 回報結果，等你確認後才推送；發現問題就停下來，不推送。

## study-tutor

- 題目只依據你提供的教材，每題標出對應章節，教材外的內容會明說。
- 逐題批改：給正確答案，答錯時講解「為什麼錯」與容易混淆的概念。
- 維護一張弱點表（章節、對錯、錯誤類型），下一輪優先出弱點章節。
- 弱點表只存在目前的對話裡；想跨對話延續，請自己存成檔案或文件，下次貼回來。

## project-progress-log

把口述或貼上的零散內容整理成固定格式：做了什麼、遇到的問題（原因與處理）、尚未解決、下一步、需要的資源或決定。

- 只記錄你說過或做過的事，不自行補充或推測。
- 預設把紀錄貼在回覆裡；你指定檔案或文件時才附加進去。
- 貼上多天紀錄時，可以整理成週報或階段摘要。

## 安裝

### Claude Code（個人，所有專案都能用）

macOS / Linux / Git Bash：

```
git clone https://github.com/Willy1012-Tu/skills.git
mkdir -p ~/.claude/skills
cp -r skills/minimal-code skills/plan-check-fix-rules skills/resume-cli-after-reset \
      skills/github-publish-check skills/study-tutor skills/project-progress-log ~/.claude/skills/
```

Windows PowerShell：

```
git clone https://github.com/Willy1012-Tu/skills.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Get-ChildItem skills -Directory | Where-Object { Test-Path "$($_.FullName)\SKILL.md" } |
  ForEach-Object { Copy-Item -Recurse $_.FullName "$HOME\.claude\skills\" }
```

只想挑幾個的話，只複製那幾個資料夾即可。只想在單一專案使用，改放到該專案的 `.claude/skills/` 底下。裝好之後重新開一個對話。

### claude.ai 網頁版與手機 app

把想用的 skill 資料夾（裡面要有 `SKILL.md`）壓縮成 .zip，到網頁版的設定裡上傳為帳號 skill。上傳到帳號的 skill，網頁版和手機 app 登入同一個帳號都能用。設定頁的選單名稱可能隨版本調整，找不到時請看 Anthropic 的最新說明。

注意：只放在電腦本機（例如 `~/.claude/skills`）的 skill 不會自動同步到網頁版或手機。

## 使用方式

大多數情況下不需要特別做什麼，skill 會依照 `SKILL.md` 開頭 `description` 的描述自動判斷要不要套用。也可以直接點名，例如對 Claude 說「套用思考守則」。

## 自訂

直接修改 `SKILL.md` 的內容即可。檔案開頭 `---` 之間的 `name` 和 `description` 決定這個 skill 叫什麼、什麼時候被觸發，改 `description` 會改變觸發的時機，修改時請留意。

## 目錄結構

```
skills/
├── README.md
├── LICENSE
├── .gitignore
├── minimal-code/SKILL.md
├── plan-check-fix-rules/SKILL.md
├── resume-cli-after-reset/SKILL.md
├── github-publish-check/SKILL.md
├── study-tutor/SKILL.md
└── project-progress-log/SKILL.md
```

## 授權

MIT，見 [LICENSE](LICENSE)。
