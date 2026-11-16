# AGENTS.md - MedInfo Wiki (醫療資訊與臨床實務知識庫) 開發規範

## 1. 專案目的 (Project Purpose)
本專案為「MedInfo Wiki 醫療資訊與臨床實務知識庫」，旨在建立一套結構化、易讀且可持續擴充的跨領域教學講義網站。內容涵蓋：
- **臨床檢驗判讀 (`lab`)**：常見血液與生化指標、臨床意義與異常處置。
- **醫管與輔具法規 (`policy`)**：台灣醫療法規、CNS 15390 輔具分類與長照補助規範。
- **智慧醫療技術 (`ai`)**：醫療大語言模型、RAG 檢索增強生成、臨床決策支援系統 (CDSS)。

**核心目標**：任何 AI Agent 讀取本文件後，皆須在不破壞既有版面與風格的前提下，依照相同的模組化結構新增主題頁面或講義單元。

## 2. 網頁整體風格與視覺規範 (Design System & Style Guide)
網站採「現代專業醫療知識庫 (Clean Clinical Documentation)」風格，強調高對比度、表格結構化與低視覺疲勞。AI 新增或修改元件時，**禁止自創隨機色碼**，必須嚴格使用以下 CSS 變數：

- **主色調 (Primary Medical Teal)**：`--primary: #0f766e`（深醫療青，用於主標題強調、啟用狀態與重點邊框）
- **次要強調色 (Accent Cyan)**：`--primary-light: #f0fdfa`（淺青底色，用於定義區塊背景與 Hover 狀態）
- **背景與表面色 (Background & Surface)**：
  - 網頁背景：`--bg-main: #f8fafc`
  - 卡片與內容區背景：`--bg-surface: #ffffff`
  - 側邊導覽列背景：`--bg-sidebar: #0f172a`（深藍灰，提供清晰導覽對比）
- **語意狀態色 (Semantic Badges)**：
  - 正常/建議 (`--status-normal`)：文字 `#166534` / 背景 `#dcfce7`
  - 警示/注意 (`--status-warn`)：文字 `#9a3412` / 背景 `#ffedd5`
  - 危急/重點 (`--status-alert`)：文字 `#991b1b` / 背景 `#fee2e2`
- **字體與排版 (Typography)**：
  - 預設字體：`'Noto Sans TC', -apple-system, BlinkMacSystemFont, sans-serif`
  - 程式碼與檢驗數值字體：`'JetBrains Mono', monospace`
  - 卡片圓角統一為 `12px`，邊框統一為 `1px solid #e2e8f0`。

## 3. 目錄結構與檔案命名規範 (Directory & Naming Conventions)
專案採用輕量化靜態部署架構（相容於 GitHub Pages 免編譯環境）：

```text
project-root/
├── AGENTS.md              # AI 開發與擴充規範文件（本文件）
├── index.html             # 知識庫主入口與單頁模組化講義檢視器
└── pages/                 # (擴充獨立頁面時使用) 存放各主題獨立 HTML 講義
    ├── lab-[topic].html
    ├── policy-[topic].html
    └── ai-[topic].html
```

### 命名規則 (Naming Rules)
1. **主題識別碼 (Topic ID) 與檔名**：一律使用小寫英數與連字號 (`kebab-case`)，並冠上三大分類前綴：
   - 臨床檢驗類：`lab-` 開頭（例：`lab-hemoglobin`、`lab-wbc-differential`）
   - 法規與醫管類：`policy-` 開頭（例：`policy-cns15390`、`policy-nhi-tw-drgs`）
   - 智慧醫療類：`ai-` 開頭（例：`ai-rag-healthcare`、`ai-fhir-standard`）
2. **禁止事項**：禁止使用中文檔名、空白字元、底線 (`_`) 或大寫字母作為 ID 與檔案名稱。

## 4. 頁面與講義標準結構 (Standard Page & Lesson Structure)
每一篇講義單元（無論是 `index.html` 內的模組化視圖，或 `pages/` 下的獨立頁面）都必須嚴格包含以下 **4 個標準 DOM 區塊**，順序不可調換：

### 區塊 A：單元標頭區 (`.lesson-header`)
- **分類標籤 (`.category-badge`)**：標示所屬領域（`臨床檢驗` / `醫管法規` / `智慧醫療`）。
- **單元主標題 (`h1.lesson-title`)**：清楚寫出中英文專有名詞。
- **元資料列 (`.lesson-meta`)**：包含「單元代碼 (ID)」、「最後更新日期 (YYYY-MM-DD)」與「核心關鍵字標籤」。

### 區塊 B：第一段 — 核心定義與背景卡 (`.section-definition`)
- 使用 `.callout-card` 樣式呈現。
- 必須在 150 字內回答：「這是什麼？」以及「為什麼在臨床或醫療資訊中很重要？」。
- 包含 3 個重點指標小卡 (`.metric-grid` > `.metric-card`)，快速列出基準值、核心標準或架構關鍵數據。

### 區塊 C：第二段 — 結構化判讀／比較矩陣表 (`.section-matrix`)
- 每個單元**必須至少包含一張結構化表格 (`.wiki-table`)**。
- 表格欄位須為 4 欄設計（例如：`分類/狀態`、`數值或技術規格`、`臨床/實務意義`、`處置或系統設計建議`）。
- 表格內的狀態標籤須套用 `.badge-normal`、`.badge-warn` 或 `.badge-alert`。

### 區塊 D：第三段 — 實務常見問題與延伸應用 (`.section-qa`)
- 包含至少 2 題針對該主題的進階問答 (`.qa-item`)。
- 每個 `.qa-item` 須包含問題標題 (`.qa-question`) 與具體解答 (`.qa-answer`)，解答須結合臨床實務或資訊系統實作情境。

## 5. AI 修改與新增內容守則 (Rules for AI Agents)
當使用者要求「新增一個主題頁面／單元」或「修改既有內容」時，AI 必須遵守以下工作流程：

1. **步驟一：確認分類與命名**
   - 判斷新主題屬於 `lab-`、`policy-` 或 `ai-` 哪一類，並決定符合 `kebab-case` 的唯一 ID。
2. **步驟二：建立標準三段式內容**
   - 嚴格按照第 4 節的「標頭 → 核心定義卡（含 3 個指標小卡）→ 4 欄結構化表格 → 2 題以上實務 QA」撰寫內容，不可省略任何一個區塊。
3. **步驟三：同步更新導覽列與資料索引**
   - 若在 `index.html` 新增單元：必須將新單元物件加入 `WIKI_LESSONS` 資料陣列中，側邊導覽列 (`.sidebar-nav`) 與上方統計計數器會依規則自動渲染；若為靜態 HTML 列表，則須同步在對應的 `<ul class="nav-group">` 下新增 `<li>` 連結。
4. **步驟四：遵守語氣與專業正確性**
   - 一律使用**繁體中文（台灣醫療與資訊慣用術語）**（例如：使用「血紅素」而非「血紅蛋白」、使用「輔具」而非「輔助器具」、使用「檢索增強生成」）。
   - 禁止刪除或覆蓋既有的其他講義單元。
5. **步驟五：輸出前自我檢核 (Self-Check)**
   - [ ] 是否使用了規定的 CSS 變數與 Class 名稱（`.section-definition`、`.section-matrix`、`.section-qa`）？
   - [ ] 單元 ID 是否符合 `lab-*` / `policy-*` / `ai-*` 命名規範？
   - [ ] 是否包含 3 個 `.metric-card` 與至少 1 張 `.wiki-table`？
