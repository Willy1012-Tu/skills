# skills

我自己在用的 Claude skills。每個資料夾是一個獨立的 skill，裡面的 `SKILL.md` 就是全部內容，沒有額外的腳本或相依檔案。

## 目前有哪些 skill

| Skill | 一句話說明 | 什麼時候會被用到 |
|---|---|---|
| [minimal-code](minimal-code/SKILL.md) | 寫程式時只寫解決問題所需的最少程式 | 要求寫程式、改程式、寫腳本時 |
| [plan-check-fix-rules](plan-check-fix-rules/SKILL.md) | 依「規劃 → 檢查 → 修正」的順序工作，再交出答案 | 多步驟推理、分析、寫作、程式、研究類任務，或你說「套用思考守則」時 |

## minimal-code

目標是「剛好解決你說的問題，不多不少」。最少不等於不正確：程式仍然要能跑、結果要對。

主要規則：

- 只做被要求的事，不預留「以後可能用到」的參數、設定檔或擴充點。
- 能用一個函式解決就不寫類別，能放在一個檔案就不拆成多個。
- 優先使用語言內建功能和標準函式庫，非必要不引入新套件。
- 註解只寫「為什麼這樣做」且不明顯的地方，不解釋程式碼字面在做什麼。
- 錯誤處理只放在會合理出錯的邊界（使用者輸入、檔案、網路）。
- 修改既有程式時，只改需要改的地方，不順手重新排版、改名或重構。
- 交出之前一定實際執行過，有邊界情況就測一下，正確性優先於短。
- 你明確要求加註解、測試、錯誤處理、拆檔或指定套件時，以你的要求為準。

## plan-check-fix-rules

處理任務時依序套用三組規則。簡單的一句話問答直接回答，不需要走完整流程。

1. **規劃（動手之前）**：先寫出要交付什麼、怎樣算完成；把資訊分成「已知」「假設」「必須查證」；評估猜錯的代價；先做最不確定的那一步。
2. **檢查（交出之前）**：逐項對照原始要求；數字重算、程式實際執行；對關鍵結論問「如果這是錯的，最可能錯在哪」；標明信心程度。
3. **修正（發現問題之後）**：先找根本原因，不只修表面症狀；修完重跑全部檢查；同一個方法失敗兩次就換方法；做不到就明說，並交代已完成的部分。

輸出原則：結論先行、事實與推測分開、無法驗證的內容標示為未驗證。

## 安裝

### Claude Code（個人，所有專案都能用）

macOS / Linux / Git Bash：

```
git clone https://github.com/Willy1012-Tu/skills.git
mkdir -p ~/.claude/skills
cp -r skills/minimal-code skills/plan-check-fix-rules ~/.claude/skills/
```

Windows PowerShell：

```
git clone https://github.com/Willy1012-Tu/skills.git
New-Item -ItemType Directory -Force "$HOME\.claude\skills" | Out-Null
Copy-Item -Recurse skills\minimal-code, skills\plan-check-fix-rules "$HOME\.claude\skills\"
```

只想在單一專案使用的話，改放到該專案的 `.claude/skills/` 底下。裝好之後重新開一個對話。

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
├── minimal-code/
│   └── SKILL.md
└── plan-check-fix-rules/
    └── SKILL.md
```

## 授權

MIT，見 [LICENSE](LICENSE)。
