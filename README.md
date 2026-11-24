# 📐 統計學黑板粉筆風學習圖卡 Stat Chalk Cards

把統計學概念做成**風格統一的黑板粉筆風學習圖卡**。
寫一份 Markdown 筆記，就能依用途輸出 **圖卡（A4 直式）／海報（A3 橫式）／封面（16:9）** 三種尺寸的單頁 HTML 與 PNG。
專案附有一份 `agent.md`，可以交給 AI 代理人（Claude、Codex、Copilot 等）照固定流程自動產出。

| 01 平均數與中位數｜圖卡 | 02 常態分布｜海報 | 03 信賴區間｜封面 |
|:---:|:---:|:---:|
| <img src="images/preview-01-card.png" width="230"> | <img src="images/preview-02-poster.png" width="330"> | <img src="images/preview-03-cover.png" width="300"> |

---

## ✨ 特色

- **一份筆記、三種用途**：同一個主題可選擇做成圖卡、海報或封面（可複選），檔名自動標註用途。
- **風格一致**：黑板底、木框、粉筆顆粒、六色粉筆、手寫字型全部集中在 `templates/base.css`，所有尺寸共用。
- **筆記格式自由**：只要寫 front matter 與 `## 標題` 分塊；比較型、流程型、單張大圖型都能排版。
- **支援數學公式與圖解**：KaTeX 渲染 LaTeX 公式；內嵌 SVG 會自動套上粉筆質感濾鏡；Markdown 表格會變成粉筆線表格。
- **離線、可重現的 PNG**：輸出時字型與 KaTeX 從本機 `node_modules` 載入，不需連網、每次結果一致。
- **台灣標準字形**：中文用霞鶩文楷 TC，並以芫荽字型修正「為」等非台灣標準字形。

---

## 📁 檔案結構與用途

```
stat-chalk-cards/
├── README.md                 ← 本說明
├── agent.md                  ← AI 代理人的工作規範（流程、筆記寫法、版面與風格規則）
├── build.py                  ← 轉換：notes/*.md → html/*-<用途>.html
├── render.py                 ← 截圖：html/*.html → png/*.png（依用途自動決定解析度、壓縮）
├── requirements.txt          ← Python 套件
├── package.json              ← Node 套件（本機字型與 KaTeX，供離線輸出 PNG）
├── package-lock.json
│
├── notes/                    ← ① 筆記（Markdown），一個主題一個檔
│   ├── 01-mean-median.md
│   ├── 02-normal-distribution.md
│   └── 03-confidence-interval.md
│
├── layouts/                  ← 自訂版面（選用）：筆記不是標準結構時，指定每個區塊放哪裡
│   ├── 01-mean-median-圖卡.html
│   └── 02-normal-distribution-海報.html
│
├── html/                     ← ② 單頁 HTML（可直接用瀏覽器開啟、分享）
│   ├── 01-mean-median-圖卡.html
│   ├── 02-normal-distribution-海報.html
│   └── 03-confidence-interval-封面.html
│
├── png/                      ← ③ PNG 圖卡（最終成品）
│   ├── 01-mean-median-圖卡.png
│   ├── 02-normal-distribution-海報.png
│   └── 03-confidence-interval-封面.png
│
└── templates/                ← 共用風格與標準版型
    ├── base.css              ← 唯一定義顏色、字型、黑板材質、框線的地方
    ├── card.html             ← 標準版型：圖卡
    ├── poster.html           ← 標準版型：海報
    ├── cover.html            ← 標準版型：封面
    ├── note-template.md      ← 「標準結構」筆記範本（參考用）
    └── partials/             ← 共用片段：字型與 KaTeX 載入、粉筆濾鏡、底線、粉筆槽、公式渲染腳本
```

**命名規則**：`NN-英文代稱`，例如 `03-confidence-interval`。
筆記不分用途（`notes/03-confidence-interval.md`），輸出檔在結尾標註用途（`-圖卡`、`-海報`、`-封面`）。

---

## 三種用途

| 用途 | 檔名尾碼 | 畫布 | PNG 輸出 | 放什麼 | 適合 |
|---|---|---|---|---|---|
| 圖卡 | `-圖卡` | A4 直式 | 1588 × 2246 | 一個概念的完整重點 | 講義、社群分享、列印 |
| 海報 | `-海報` | A3 橫式 | 3176 × 2246 | 最完整，含延伸內容，字最大 | 教室布告欄、投影 |
| 封面 | `-封面` | 16:9 | 1920 × 1080 | 標題、一句話、代表圖、口訣 | 簡報首頁、影片縮圖 |

---

## 安裝

需要 Python 3.9+ 與 Node.js 18+。

```bash
git clone https://github.com/<你的帳號>/stat-chalk-cards.git
cd stat-chalk-cards

pip install -r requirements.txt
python -m playwright install chromium
npm install          # 安裝本機字型與 KaTeX
```

---

## 搭配 AI 使用

`agent.md` 是寫給 AI 的工作規範，規定了從主題到成品的完整流程，確保每張圖卡內容正確、風格一致。

### 使用方式

1. 用支援讀寫檔案的 AI 工具開啟這個資料夾（例如 Claude、Claude Code、Codex、GitHub Copilot Agent）。
2. 請它先讀 `agent.md`，再告訴它主題，例如：

   > 請依照 agent.md，幫我做「標準差」。

3. 代理人會照以下流程進行：

```
你提出主題
   │
   ▼
① 代理人依主題選擇最適合的結構，整理筆記 notes/NN-slug.md
   │
   ▼
② 代理人給你看筆記，並詢問：
     ・內容可以嗎？要改哪裡？
     ・要做成哪種用途？（圖卡／海報／封面，可複選）
   │        ↖ 需要修改就只改 md，再確認一次
   ▼
③ 依選定用途決定版面（標準版型或自訂 layouts/）
   │
   ▼
④ 只產生選定用途的 HTML 與 PNG
   │
   ▼
⑤ 代理人逐張檢查 PNG（重疊、溢出、公式渲染）後交付
```

也可以一次指定多個主題與用途：

> 請依照 agent.md 做：01 平均數／中位數做圖卡、02 常態分布做海報、03 信賴區間做封面。

### agent.md 規範了什麼

| 章節 | 內容 |
|---|---|
| 三種用途與檔名 | 尺寸、解析度、檔名標註規則 |
| 工作流程 | 先寫筆記 → 確認內容與用途 → 才轉檔；未選用途前不可自行產出 |
| 筆記寫法 | front matter、自由分塊、參考結構（比較型、流程型、決策型、大圖型、速查型）、粉筆色標記語法 |
| 自訂版面 | 如何複製版型、插槽語法、排版範例與原則 |
| 風格一致性 | 版面裡**能改**的（排列、尺寸）與**不能改**的（顏色、字型、材質） |
| 內容品質 | 數字需用程式驗算、區分樣本／母體符號、常見誤解須真實 |

---

