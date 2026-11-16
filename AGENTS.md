# AGENTS.md / CLAUDE.md — 專案開發與知識庫維護憲章

> **給 AI 協作者的核心指令**：
> 你是本專案的「知識庫管理員（Knowledge Curator）」兼「前端展示工程師」。
> 在執行任何新增、修改或重構任務前，請完整閱讀本文件。你的核心目標是**讓這座知識庫與公開網站能在嚴格溯源、風格一致的前提下持續擴充與演進**。

---

## 1. 專案目的（Project Purpose）

本專案是一個結合 **LLM Wiki 結構化知識管理模式** 與 **GitHub Pages 靜態公開網站** 的個人研究與世界觀知識庫。
1. **知識沉澱**：將零散的原始文獻（論文、Fandom 百科條目、技術文章）轉化為結構化、可交叉引用的 Markdown 知識卡片。
2. **公開展示**：透過根目錄的 `index.html` 與 `presentations/` 下的互動式簡報，讓任何訪客都能透過 GitHub Pages 直接在瀏覽器中檢索知識、閱讀筆記與演示簡報。
3. **人機協作延續性**：確保每一次新增資料或修改網頁時，資料來源可追溯、檔案結構不混亂、視覺風格維持高度統一。

---

## 2. 網頁整體風格（Visual Design System）

本專案所有網頁（包含 `index.html` 主站與 `presentations/` 簡報）預設採用 **「偉大航道・古典海圖羊皮紙風（Parchment Log Pose Style）」**，修改或新增頁面時必須嚴格沿用以下 CSS 變數與設計語彙，**禁止擅自改為現代 SaaS 紫藍漸層或預設白底黑字**：

- **色彩系統（Color Palette）**：
  - 外部底色（`--bg-page` / `--bg-outer`）：`#EFE7D6` 或 `#DCD0B8`（陳年羊皮紙底色）
  - 畫布／紙張主色（`--bg-paper` / `--bg-slide`）：`#F8F2E4` 或 `#F7F0E0`（暖米白海圖紙面）
  - 卡片與表格底色（`--bg-card`）：`#F1E7D0` 或 `#EFE4CC`
  - 主要強調色（`--accent-red` / `--accent-main`）：`#B53028`（草帽赤紅，用於標題重點、Active 標籤、主要按鈕）
  - 次要強調色（`--accent-gold` / `--accent-sub`）：`#9E6E2B`（羅盤古銅金，用於分隔線、邊框飾條）
  - 內文主色（`--text-main`）：`#2A1B10`（深棕墨水色）；次要內文：`#573F2B`
  - 邊框線條（`--border-color`）：`#C7B28E`
- **背景紋理與邊框**：
  - 背景需保留 `40px × 40px` 的經緯度網格細線（`rgba(138, 102, 56, 0.09)`）。
  - 簡報頁面（`presentations/*.html`）需維持 16:9 寬螢幕比例、雙線古典內框，並支援「偉大航道海圖 / 海軍實驗室白底 / 新世界暗黑」三套主題切換。
- **排版與數學公式**：
  - 標題字體優先使用襯線體（`"Noto Serif TC", "Songti TC", Georgia, serif`）；內文使用無襯線體（`"Noto Sans TC", "PingFang TC", sans-serif`）。
  - 所有數學公式與演算法推導必須使用 **KaTeX**（`$$...$$` 與 `$...$`）渲染，嚴禁使用純文字替代複雜公式。
  - 資料對比一律使用「學術三線表（Booktabs 風格：頂線與底線 `2px solid`，表頭分隔線 `1.5px solid`，無垂直格線）」。

---

## 3. 頁面結構（Page Architecture）

### A. 主入口網站（`index.html`）
採用單頁式無框架（Vanilla HTML/CSS/JS）架構，確保零建置步驟即可在 GitHub Pages 運行，包含以下 4 個核心區塊：
1. **頂部黏性導航列（Sticky Header）**：左側為 `LOG POSE WIKI` 印章與站名；右側為分類切換分頁（全部條目、論文精讀、核心技術、模型架構、深度綜述、專題簡報）。
2. **站點數據儀表板（Stats Grid）**：顯示各分類（`papers`、`methods & architectures`、`syntheses`、`presentations`）的即時條目數量。
3. **即時檢索與標籤過濾列（Filter Bar）**：提供關鍵字搜尋框（比對標題與摘要）與 `#tag` 標籤篩選器。
4. **雙欄工作區（Workspace）**：
   - 左側（`360px`）：條目卡片列表（`.entry-list`）。
   - 右側（`1fr`）：詳情閱讀器（`.reader-pane`），渲染完整 HTML 內容、KaTeX 公式、三線表與溯源路徑。

### B. 互動式簡報頁面（`presentations/*.html`）
- 獨立單檔 HTML，頂部具備主題切換器、講者筆記開關（`N` 鍵）、PDF 列印匯出與上一頁／下一頁按鈕（使用 `DOMContentLoaded` 與標準 `addEventListener` 綁定事件，確保按鈕與鍵盤 `←` `→` `Space` 穩定運作）。

---

## 4. 檔案應該如何命名與放置（Directory & Naming Conventions）

```text
/
├── index.html                                     # GitHub Pages 網站主入口（唯一定位入口）
├── AGENTS.md                                      # AI Agent 操作規範（與 CLAUDE.md 同步）
├── CLAUDE.md                                      # Claude Code 專用規範（與 AGENTS.md 同步）
├── README.md                                      # 專案說明與部署教學
├── .github/workflows/pages.yml                    # GitHub Actions 自動部署設定
├── raw/                                           # 【唯讀層】原始資料，寫入後嚴禁修改
│   ├── papers/                                    # 命名：YYYY-<FirstAuthor>-<ShortTitle>.md
│   └── articles/                                  # 命名：YYYY-MM-DD-<TopicSlug>.md
├── wiki/                                          # 【知識層】結構化 Markdown 條目
│   ├── index.md                                   # 知識庫總目錄（使用 [[Wikilink]] 連結）
│   ├── log.md                                     # 知識庫變更日誌（按時間倒序記錄）
│   ├── papers/                                    # 單篇論文精讀卡片（YYYY-<Author>-<Title>.md）
│   ├── architectures/                             # 模型與系統架構卡片（<ArchitectureName>.md）
│   ├── methods/                                   # 演算法與技術方法卡片（<MethodName>.md）
│   └── syntheses/                                 # 跨文獻深度綜述（YYYY-MM-DD-<Topic>.md）
├── notes/                                         # 【研究筆記層】嚴格基於 wiki/ 的問答與筆記
├── slide_outlines/                                # 【簡報腳本層】YYYY-MM-DD-<Topic>-slides.md
└── presentations/                                 # 【簡報網頁層】YYYY-MM-DD-<Topic>.html
```

**命名鐵律**：
- 所有資料夾與檔名一律使用 **英文、數字與連字號（`-`）**，**禁止出現空格或中文字元**，以免 GitHub Pages 產生 URL 編碼斷鏈。
- 日期統一採用 ISO 格式：`YYYY-MM-DD`。

---

## 5. 新增內容時要遵守的格式（Content Schemas）

### 規範 A：新增 `wiki/` Markdown 條目格式
每個放入 `wiki/` 子目錄的 `.md` 檔案必須包含以下結構：
```markdown
# <條目標題>

- **分類**：`papers` | `methods` | `architectures` | `syntheses`
- **標籤**：`#tag1` `#tag2`
- **原始溯源**：`[[raw/papers/檔名.md]]` 或外部來源 URL

## 1. 核心摘要
（3～5 句話精準概括核心貢獻或世界觀設定）

## 2. 關鍵機制與公式
（使用條列式說明，數學公式使用 `$$...$$`）

## 3. 實驗數據／結構對照
（使用標準 Markdown 表格呈現）
```

### 規範 B：同步更新 `index.html` 的 `wikiData` 陣列
每當在 `wiki/` 新增條目或更新站點統計時，**必須同步在 `index.html` 的 `<script>` 內 `wikiData` 陣列新增對應物件**，確保網頁能立即瀏覽新內容：
```javascript
{
  id: "unique-kebab-case-id",
  category: "papers", // 僅限: papers | methods | architectures | syntheses | presentations
  categoryLabel: "PAPERS // 論文單篇精讀",
  title: "完整條目標題",
  tags: ["tag1", "tag2"],
  source: "wiki/papers/檔名.md",
  summary: "顯示於列表與閱讀器頂部的簡短摘要。",
  html: `
    <h3>1. 小節標題</h3>
    <p>內文內容（支援 $inline$ 與 $$display$$ KaTeX 公式）</p>
  `
}
```
> 同時記得同步更新 `index.html` 頂部 `.stats-grid` 中的對應數字卡片！

---

## 6. AI 修改或新增內容時需要遵守的規則（Agent Rules）

1. **原始資料不可變原則（Immutable `raw/`）**：
   - 任何存入 `raw/` 的檔案視為歷史快照，**絕對禁止編輯、覆寫或刪除**。若需補充解讀，請寫在 `wiki/` 中。
2. **嚴格知識邊界與誠實標註（Strict Grounding）**：
   - 在撰寫 `wiki/`、`notes/` 或 `presentations/` 時，**只能依據 `raw/` 或 `wiki/` 中已存在的資料進行綜合整理**，嚴禁憑空捏造數據、話數或實驗結果。
   - 若現有資料不足以回答某個子問題，必須在該處明確標註 **`[Wiki 知識缺口]`**，並在文末或簡報末頁列出「建議補充文獻」供使用者攝取。
3. **雙檔同步原則（`AGENTS.md` $\leftrightarrow$ `CLAUDE.md`）**：
   - 若未來修改了本規範文件的任何規則，**必須同時更新 `AGENTS.md` 與 `CLAUDE.md`**，保持兩份檔案內容 100% 一致。
4. **修改 `index.html` 的安全守則**：
   - 保留單頁免編譯（No-Build）特性，**不要引入 React、Vue、Webpack 或需要 `npm install` 的架構**，確保推送到 GitHub `main` 分支後 GitHub Pages 能直接渲染。
   - 新增互動功能時，所有 DOM 事件必須綁定在 `DOMContentLoaded` 之後或使用清晰的函數命名，避免變數污染與按鈕無反應。
5. **每次任務完成後的標準檢查清單（Definition of Done）**：
   - [ ] 新資料是否已完整備份至 `raw/`？
   - [ ] 結構化筆記是否已寫入 `wiki/` 並更新 `wiki/index.md`？
   - [ ] `index.html` 的 `wikiData` 與統計數字是否已同步更新？
   - [ ] 若有生成新簡報，是否已放入 `presentations/` 並在 `index.html` 建立開啟連結？
