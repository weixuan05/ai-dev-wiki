# AGENTS.md - AI-Dev-Wiki 專案開發與擴充規範

## 1. 專案目的 (Project Purpose)
本專案為「AI-Dev-Wiki（AI 與現代開發實戰筆記庫）」，是一個純靜態、輕量化、可持續擴充的個人技術知識庫網站。
核心目標是將複雜的 AI 技術（如 RAG、AI Agent、Prompt Engineering）整理成結構一致、易於閱讀的網頁講義，並讓 AI 助理能依據本文件的規範，快速新增或維護筆記頁面，無需每次重新設計排版。

---

## 2. 目錄結構與檔案命名規則 (Directory & Naming Conventions)

專案採用純靜態網頁架構（HTML5 + CSS3），不使用任何複雜打包工具，確保可直接透過 GitHub Pages 部署：

```text
/ (專案根目錄)
├── index.html                  # 網站首頁（顯示專案簡介與所有筆記章節卡片目錄）
├── AGENTS.md                   # AI 開發規範文件（本檔案）
├── assets/
│   └── style.css               # 全站統一樣式表（嚴禁在其他檔案寫 inline style）
└── notes/                      # 所有筆記內頁存放資料夾
    ├── 01-rag-architecture.html
    └── 02-ai-agent-patterns.html
```

### 命名規則
1. **筆記內頁**：一律放置於 `notes/` 資料夾下。
2. **檔名格式**：必須採用 `兩位數編號-英文小寫kebab-case.html`（例如：`03-prompt-engineering.html`）。
3. **路徑規範**：所有連結必須使用**相對路徑**（首頁連內頁用 `notes/xx.html`，內頁連首頁用 `../index.html`，內頁載入 CSS 用 `../assets/style.css`），以確保在 GitHub Pages 子路徑下正常運作。

---

## 3. 視覺設計與風格規範 (Design & Styling Rules)

全站樣式集中管理於 `assets/style.css`，AI 新增頁面時必須沿用既有 CSS Class，禁止自行發明新樣式表：

- **色彩系統 (CSS Variables)**：
  - 主色調 (`--primary`)：`#2563eb`（科技藍，用於標題強調、按鈕與連結）
  - 背景色 (`--bg-body`)：`#f8fafc`（極淺灰藍，降低閱讀疲勞）
  - 卡片底色 (`--bg-card`)：`#ffffff`（純白，搭配細邊框 `#e2e8f0`）
  - 文字主色 (`--text-main`)：`#1e293b`（深石板灰）
  - 提示框語意色：重點提示 `.callout-tip`（綠色系）、注意事項 `.callout-warning`（琥珀黃色系）。
- **字體與排版**：
  - 使用系統無襯線字體（Noto Sans TC / Inter），內文行高固定 `1.75`，單欄閱讀區最大寬度限制為 `860px`。

---

## 4. 頁面 HTML 結構模板 (Page Structure Template)

### A. 筆記內頁標準骨架 (`notes/XX-topic.html`)
每一篇新建立的筆記頁面，都必須嚴格遵守以下 HTML 結構：

1. `<head>`：載入 `../assets/style.css`，`<title>` 格式為 `章節標題 | AI-Dev-Wiki`。
2. `<header class="site-header">`：全站統一頂部導覽列（包含網站 Logo 與首頁連結）。
3. `<main class="container">`：
   - `<nav class="breadcrumb">`：麵包屑導覽（例如：`首頁 / 03. 主題名稱`）。
   - `<article class="doc-article">`：文章主體，包含 `<h1>` 主標題、`<p class="doc-meta">`（更新日期與標籤）、`<h2>` 分段標題與內容區塊。
   - `<div class="doc-pagination">`：文末「上一篇 / 返回目錄 / 下一篇」導覽按鈕。
4. `<footer class="site-footer">`：全站統一頁尾版權宣告。

### B. 內容區塊專用元件 (Content Components)
撰寫內文時，請優先使用以下標準 HTML 元件：
- **重點提示框**：`<div class="callout callout-tip"><strong>💡 核心重點：</strong>內容...</div>`
- **警告／易錯提醒框**：`<div class="callout callout-warning"><strong>⚠️ 注意事項：</strong>內容...</div>`
- **程式碼區塊**：`<pre><code>程式碼內容</code></pre>`

---

## 5. AI 新增或修改內容標準作業流程 (AI Workflow SOP)

當開發者要求「新增一篇關於 [主題] 的筆記」時，AI 必須依序執行以下 **4 個步驟**，缺一不可：

1. **檢查編號**：查看 `notes/` 目錄下現有的最大編號（例如目前為 `02`），將新檔案命名為下一號（`03-xxx.html`）。
2. **生成內頁**：複製 `notes/02-ai-agent-patterns.html` 的完整 HTML 骨架，替換標題、麵包屑、內文與元件，確保所有 CSS 與導覽連結皆使用 `../` 相對路徑。
3. **更新上一篇的「下一篇」連結**：打開前一編號的筆記檔案（`02-ai-agent-patterns.html`），將其文末 `.doc-pagination` 中的「下一篇」按鈕指向新建的 `03-xxx.html`。
4. **同步更新首頁目錄 (`index.html`)**：打開根目錄的 `index.html`，在 `<div class="notes-grid">` 區塊最下方，依照既有 `<a class="note-card">` 格式新增一張對應新筆記的章節卡片（包含編號標籤、標題、50 字內簡介與主題標籤）。

## 6. 禁止事項 (Restrictions)
- ❌ 禁止引入外部大型 CSS/JS 框架（如 Bootstrap、jQuery、React），維持純淨原生 HTML/CSS。
- ❌ 禁止在 HTML 標籤中使用 `style="..."` 行內樣式。
- ❌ 禁止使用 `/` 開頭的絕對路徑（例如 `/assets/style.css`），避免 GitHub Pages 部署破圖。
