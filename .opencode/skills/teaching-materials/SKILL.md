# 教學教材應用懶人包

## 功能
- 從 PDF 掃描檔 OCR 提取教材筆記內容
- 自動生成各章節 DOCX 教學文件（含 OMML 數學公式）
- 自動生成各章節 PDF 教材
- 上傳至 NotebookLM 建立「教材教學」筆記本
- 同步至 Obsidian 教材教學資料夾
- 建立互動式 HTML 簡報（含 Firebase 即時文字雲）
- 一鍵部署至 Firebase Hosting

## 使用方法

### 前置需求
- Python 3.8+
- pip install pypandoc-binary python-docx opencc-python-reimplemented
- Node.js + Firebase CLI（選擇性，用於部署 Hosting）

### 執行流程

#### 1. 從 PDF 提取文字內容
```bash
# 使用 NotebookLM MCP 上傳 PDF 並查詢
notebooklm source_add source_type=file file_path=教材筆記.pdf wait=true
notebooklm query query="列出所有章節與內容"
```

#### 2. 產生 DOCX 教學筆記
```bash
python convert_md_to_docx.py
```

#### 3. 轉換為 PDF
```bash
python convert_docx_to_pdf.py
```

#### 4. 上傳至 NotebookLM
```bash
# 建立筆記本並上傳各章節 PDF
notebooklm notebook_create title=教材教學
notebooklm source_add notebook_id=<ID> source_type=file file_path=output/*.pdf
```

#### 5. 同步至 Obsidian
- 將 `obsidian/` 目錄內容複製至 Obsidian Vault 的 `教材教學/` 資料夾

#### 6. 建立互動簡報
```bash
# 修改 presentation/index.html 後
firebase deploy --only hosting
```

### 目錄結構
```
教學教材資料/
├── chapters/              # 各章節 Markdown 原始檔
├── output/                # 產出 DOCX + PDF
├── presentation/          # HTML 互動簡報
│   └── index.html         # 主簡報檔（Firebase Hosting）
├── pdf_pages/             # PDF 每頁 PNG 截圖
├── formulas/              # 公式 PNG 圖片
├── obsidian/              # Obsidian Markdown 筆記
├── firebase.json          # Firebase 設定
├── database.rules.json    # Realtime Database 規則
├── convert_md_to_docx.py  # MD→DOCX 轉換腳本
├── convert_docx_to_pdf.py # DOCX→PDF 轉換腳本
├── generate_docx.py       # 舊版 DOCX 產生腳本
├── cleanup.py             # 清理腳本
└── rename_and_convert.py  # 重新命名轉換腳本
```

## 互動簡報功能
- 7 章節幻燈片瀏覽（鍵盤/觸控/滑鼠導航）
- 每章核心概念 + 公式 + 重點提示
- **Firebase 即時文字雲**：學生輸入關鍵字即時更新
- 進度條、動畫效果、自適應設計
- 部署於 Firebase Hosting

## 技術棧
- **文件生成**：Python + pypandoc + python-docx
- **簡報前端**：HTML5 + CSS3 + Vanilla JS
- **即時資料庫**：Firebase Realtime Database
- **文字雲**：wordcloud2.js
- **託管**：Firebase Hosting
- **OCR 提取**：NotebookLM MCP
- **筆記同步**：Obsidian MCP
