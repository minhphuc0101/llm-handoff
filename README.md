# llm-handoff

Ultra-light (`<200 tokens`) pure-Markdown state handoff & copy-paste context bridge for **Antigravity**, **Google Jules**, **Cursor**, **GitHub Copilot**, **Gemini in Google Sheets & Slides**, **Excel Copilot**, and **PowerPoint Copilot**—zero scripts, zero dependencies, English & Tiếng Việt.

## Why Pure `<20-Line` Markdown?

| Approach | Token Overhead | Works Outside Git (Sheets/Excel/Slides/Web Chat)? | Setup |
| :--- | :--- | :--- | :--- |
| Static `AGENTS.md` | Low | No (rules only, no live session state) | Repo file |
| Multi-file Memory Banks | High (`2,000–8,000+` tokens) | No (developer IDE only) | Multiple folders |
| Office-to-Markdown Parsers | Medium–High | One-way (`Office -> Code` only) | Python/Node runtime |
| **`llm-handoff` (This Repo)** | **`<200` tokens** | **Yes (`HANDOFF.md` + `HANDOFF!A1` + Hidden Slide)** | **Copy-paste or 1-line `curl`** |

## Repository Structure

```text
llm-handoff/
├── AGENTS.md                           # Master contract (auto-read by Antigravity, Jules & Copilot)
├── HANDOFF.md                          # Live status baton template (<20 lines, includes Constraints)
├── LICENSE                             # MIT License
├── .agents/
│   └── skills/
│       └── handoff/
│           └── SKILL.md                # Auto-discovered workspace skill for Antigravity (EN/VI)
└── README.md                           # Zero-install prompts, safe 1-line installer & bilingual SOP
```

## Quick Start — Two Ways to Use

### Option A: Zero-Install Text to Copy (Any AI Chat — No Git / Terminal Needed)
Paste this 4-line prompt into ChatGPT, Claude, or Gemini whenever wrapping up a session (or ask in Vietnamese):

```text
Summarize our current state concisely (<20 lines for code/sheets; 1-2 lines per slide for multi-slide decks — no "you're... doing..." fluff):
1. Goal & Constraints (do-not-touch files/cells) — for reporting sheets, include Time Range & Update Frequency.
2. Working / Data Lineage / Visual Assets (for sheets: `<Sheet/Block> <- <Source> via <Method>`; for slides: per-slide content + required media/screenshots, charts, tables).
3. Broken/WIP & Numbered Next Steps.
```

### Option B: 1-Line Repo Install (Non-Destructive — Preserves Existing `HANDOFF.md`)

**Bash / macOS / Linux / Git Bash:**
```bash
mkdir -p .agents/skills/handoff && curl -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/AGENTS.md -o AGENTS.md && ([ -f HANDOFF.md ] || curl -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/HANDOFF.md -o HANDOFF.md) && curl -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/.agents/skills/handoff/SKILL.md -o .agents/skills/handoff/SKILL.md
```

**PowerShell (Windows):**
```powershell
New-Item -ItemType Directory -Force .agents/skills/handoff | Out-Null; curl.exe -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/AGENTS.md -o AGENTS.md; if (!(Test-Path HANDOFF.md)) { curl.exe -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/HANDOFF.md -o HANDOFF.md }; curl.exe -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/.agents/skills/handoff/SKILL.md -o .agents/skills/handoff/SKILL.md
```

## Team Callout Cheat Sheet

| Tool | Trigger | Action |
| :--- | :--- | :--- |
| **Google Jules** | Automatic | Reads `AGENTS.md` on every task/issue and updates `HANDOFF.md` in its PR. |
| **Antigravity** | Automatic | Auto-loads `AGENTS.md` & `.agents/skills/handoff/SKILL.md`; reads `HANDOFF.md` on start and updates on wrap-up. Type `handoff for sheet`, `handoff for slides`, `cho tôi hướng dẫn gg sheet cho AI`, `cho tôi hướng dẫn slide cho AI`, or `viết hướng dẫn cho llm`. |
| **Cursor / Copilot** | `@AGENTS.md` | Mention `@AGENTS.md` at session start/end (or auto-loaded in supported versions). |
| **Gemini in Google Sheets / Excel Copilot** | `HANDOFF!A1` | Paste the exported Markdown block into cell `A1` of a `HANDOFF` tab (or open the generated `.xlsx`), then prompt: *"Read the HANDOFF tab first, then complete Next Step 1."* |
| **Gemini in Google Slides / PowerPoint Copilot** | Hidden `HANDOFF` slide / Slide 1 Notes | Paste the exported Markdown block into a skipped/hidden `HANDOFF` slide or Slide 1 Speaker Notes (or open the generated `.pptx`), then prompt: *"Read the HANDOFF slide / Slide 1 notes first, then create/update Slides 2–5."* |

## Reporting Excel / Google Sheets Checklist (`HANDOFF!A1`)

When handing off a reporting workbook, `HANDOFF!A1` must state **lineage** and **cadence** cleanly in `<20` lines:
1. **Report Window & Update Frequency:** Exact date/period range + refresh cadence (e.g., `Q4 2026 (2026-10-01..2026-12-31) | Weekly every Monday 09:00`).
2. **Sheet & Block Lineage + Method:**
   - Which sheet pulls from which sheet (`Summary` gets data from `Orders` and `Targets`).
   - Which exact block pulls from where and via which method (`Summary!B4:D8` ← `Orders!A2:G500` via `SUMIFS`; `Summary!E4:E8` ← `Targets!A2:B10` via `XLOOKUP`).
3. **Constraints & Next Steps:** Locked formula columns/ranges + numbered tasks.

**English Reporting Handoff Example (`<20 lines`):**
```markdown
# Handoff: Q4 Regional Margin Report
- **Goal & Cadence:** Q4 2026 (`2026-10-01..2026-12-31`) weekly margin report | Updated every Monday 09:00.
- **Constraints:** Do not overwrite `Orders!A:E` (raw import) or `Targets!A:B`.
- **Data Lineage (`Summary` ← `Orders`, `Targets`):**
  - `Summary!A4:A8` (Regions) ← `Targets!A2:A6` via `UNIQUE`
  - `Summary!B4:D8` (Gross, Discount, Net Margin) ← `Orders!B2:F500` via `SUMIFS` by `Region` + `Order_Date`
  - `Summary!E4:E8` (Target Margin %) ← `Targets!A2:B6` via `XLOOKUP`
- **Next Steps:**
  1. Add `Summary!F4:F8` variance (`Actual_Pct - Target_Pct`) and conditional red highlight if `< 0`.
  2. Flag `Orders` rows where `Net_Margin_USD / Gross_USD < 0.15` as `"REVIEW"` in `Orders!G2:G500`.
```

## Slides / PowerPoint Template Checklist (`Hidden HANDOFF Slide / Slide 1 Notes`)

When creating or handing off a `.pptx` / Google Slides template, state clearly per slide:
1. **What to add into the slide:** Title, key takeaway, and target audience.
2. **Exact visual assets needed:** Required **media/screenshots** (what screen/product visual + source path), **charts** (chart type + metric + source sheet/range), and **tables** (columns/filter + source).
3. **Scales With Slide Count (No `<20-Line` Cap):** Keep `~1–2` concise lines per slide; for larger decks (`10+` slides), paste into **Slide 1 Speaker Notes** (or per-slide Speaker Notes) rather than cramming onto a single hidden slide.

**English Slide Template Handoff Example (scales with slide count):**
```markdown
# Handoff Slides: Q4 Regional Margin Review (Template)
- **Goal & Audience:** 4-slide Finance executive review (`Q4 2026`) | Keep 16:9 brand layout & placeholder font sizes.
- **Per-Slide Content & Media/Chart/Table Plan:**
  - **Slide 1 (Cover & KPI):** Add Q4 headline (`$5,237.5` total net margin) | `[Media: company_logo.png]`
  - **Slide 2 (Regional Performance):** Add 1-line insight on APAC lead | `[Chart: Clustered Bar of Summary!A4:D8 (Gross vs. Net Margin by Region)]`
  - **Slide 3 (Low-Margin Review):** Add root-cause bullets for `<15%` orders | `[Table: Orders!A2:G500 filtered where Low_Margin_Flag="REVIEW"]`
  - **Slide 4 (Action Plan & Proof):** Add discount cap proposal (`max 15%`) | `[Screenshot: pricing_portal_cap.png from QA env]`
- **Next Steps:**
  1. Populate Slide 2 chart and Slide 3 filtered table from `Summary` and `Orders`.
  2. Insert `pricing_portal_cap.png` screenshot into Slide 4 right-hand placeholder.
```

## Team Workflow Summary (Repo ↔ Sheets/Excel ↔ Slides/PPTX)

1. **Code → Sheet/Excel (`handoff for sheet`):**
   - Developer finishes coding in Antigravity/Cursor and types `handoff for sheet` (or asks the agent to write an `.xlsx` workbook with a `HANDOFF` tab).
   - Agent updates `HANDOFF.md` in Git and outputs a `<20-line` self-contained Markdown summary in chat.
   - Developer copies that block and pastes it into cell `A1` of a tab named `HANDOFF` in Google Sheets or Excel.

2. **Working inside Google Sheets / Excel:**
   - Teammate opens Gemini in Google Sheets (or Excel Copilot) and prompts:
     > *"Read the HANDOFF tab first to understand the project rules, lineage, and context, then help me complete Next Steps 1–2."*

3. **Sheet/Excel → Code (`Sync HANDOFF.md`):**
   - When finished in Sheets/Excel, teammate asks the spreadsheet AI:
     > *"Update the text in HANDOFF!A1 with what we just completed and the next steps for the developer."*
   - Teammate sends that block back to the developer, who pastes it into Antigravity with: `Sync HANDOFF.md with this update.`

4. **Code/Sheet → Presentation (`handoff for slides` / `handoff for presentation`):**
   - Developer types `handoff for slides` (or asks the agent to generate a `.pptx` with a hidden `HANDOFF` slide) and pastes the slide handoff block (scales with slide count) into Slide 1 Speaker Notes or a skipped/hidden `HANDOFF` slide.
   - Teammate opens Gemini in Google Slides (or PowerPoint Copilot) and prompts:
     > *"Read the HANDOFF slide / Slide 1 notes first, then create or update Slides 2–5."*

---

## Tiếng Việt — Mô tả & Câu lệnh Bàn giao

### Mô tả dự án
`llm-handoff` là giao thức bàn giao trạng thái siêu nhẹ (`<200 tokens`) thuần Markdown giúp các AI coding agent (**Antigravity**, **Google Jules**, **Cursor**, **GitHub Copilot**), AI bảng tính (**Gemini trong Google Sheets**, **Copilot trong Excel**) và AI thuyết trình (**Gemini trong Google Slides**, **Copilot trong PowerPoint `.pptx`**) chia sẻ chung một nguồn sự thật qua `HANDOFF.md`, `HANDOFF!A1` và slide `HANDOFF` ẩn. Thay vì viết prompt dài kiểu *"Bạn là lập trình viên... hiện tại bạn đang làm..."*, nhóm chỉ cần dùng trạng thái khai báo rõ ràng: **Mục tiêu**, **Ràng buộc**, **Luồng dữ liệu (với file báo cáo)**, **Phần đã chạy/Lỗi**, và **Bước tiếp theo**.

### Bảng câu lệnh Tiếng Việt (Callouts)

| Giai đoạn | Nơi gõ lệnh | Câu lệnh mẫu (Tiếng Việt) |
| :--- | :--- | :--- |
| **Dùng nhanh không cần cài (Copy-Paste)** | ChatGPT / Claude / Gemini Web | `"Tóm tắt trạng thái dự án <20 dòng (mục tiêu, ràng buộc, luồng dữ liệu sheet/block nếu là báo cáo, việc đã xong, bước tiếp theo) để bàn giao cho AI khác."` |
| **Cài đặt vào dự án** | Antigravity / Cursor | `"Cài đặt https://github.com/minhphuc0101/llm-handoff vào dự án này"` |
| **Tạo / cập nhật `HANDOFF.md` cho LLM khác** | Antigravity / Cursor | `"viết hướng dẫn cho llm"` hoặc `"viết md hướng dẫn"` hoặc `"tạo file hướng dẫn AI"` |
| **Xuất khối Markdown (`Code → Sheet/Excel`)** | Antigravity / Cursor | `"cho tôi hướng dẫn gg sheet cho AI"` (hoặc `"bàn giao cho sheet"` / `"bàn giao qua Excel"`) |
| **Xuất trực tiếp file `.xlsx`** | Antigravity / Cursor | `"Cập nhật HANDOFF.md và tạo file Excel bàn giao có tab HANDOFF ở ô A1"` |
| **Xuất khối Markdown cho Slide (`Code/Sheet → Slide/PPTX`)** | Antigravity / Cursor | `"cho tôi hướng dẫn slide cho AI"` (hoặc `"bàn giao cho slide"` / `"handoff for slides"`) |
| **Xuất trực tiếp file `.pptx`** | Antigravity / Cursor | `"Cập nhật HANDOFF.md và tạo file PowerPoint (.pptx) có slide HANDOFF ẩn ở đầu"` |
| **Làm việc trong Sheet / Excel** | Gemini (Sheets) / Copilot (Excel) | `"Đọc tab HANDOFF trước để nắm mục tiêu, kỳ báo cáo, luồng dữ liệu và quy tắc, sau đó thực hiện Bước tiếp theo 1–2."` |
| **Làm việc trong Google Slides / PowerPoint** | Gemini (Slides) / Copilot (PPTX) | `"Đọc slide HANDOFF (hoặc ghi chú Slide 1) trước để nắm mục tiêu, số liệu và bố cục, sau đó hoàn thiện các slide tiếp theo."` |
| **Cập nhật khi làm xong (`Sheet/Slide → Code`)** | Sheets / Excel / Slides / PPTX | `"Cập nhật lại ô HANDOFF!A1 (hoặc slide HANDOFF) với những việc vừa hoàn thành và bước tiếp theo cho dev."` |
| **Đồng bộ lại vào Code** | Antigravity / Cursor | `"Đồng bộ HANDOFF.md theo nội dung này: <dán nội dung HANDOFF>"` |

### Ví dụ khối bàn giao Tiếng Việt (Không cần *"Bạn là... đang làm..."*)

**1. Bàn giao file Báo cáo Excel / Google Sheets (`HANDOFF!A1`, `<20 dòng` — ghi rõ kỳ báo cáo, tần suất & nguồn từng sheet/block):**
```markdown
# Bàn giao Báo cáo: Biên lợi nhuận Q4
- **Mục tiêu & Tần suất:** Tổng hợp biên lợi nhuận Q4 2026 (`2026-10-01..2026-12-31`) | Cập nhật mỗi thứ Hai 09:00.
- **Ràng buộc:** Không sửa tay dữ liệu gốc ở `Raw_Orders!A:F` và bảng chỉ tiêu `Targets!A:B`.
- **Luồng dữ liệu (`Summary` lấy từ `Raw_Orders` & `Targets`):**
  - `Summary!A4:A8` (Khu vực) ← `Targets!A2:A6` bằng `UNIQUE`
  - `Summary!B4:D8` (Doanh thu, Chiết khấu, Lợi nhuận ròng) ← `Raw_Orders!B2:F500` bằng `SUMIFS` theo `Region` + `Order_Date`
  - `Summary!E4:E8` (Chỉ tiêu %) ← `Targets!A2:B6` bằng `XLOOKUP`
- **Bước tiếp theo:**
  1. Tính cột chênh lệch `Summary!F4:F8` (`Thực tế % - Chỉ tiêu %`) và tô đỏ nếu `< 0`.
  2. Ở `Raw_Orders!G2:G500`, gắn cờ `"REVIEW"` nếu `Lợi nhuận / Doanh thu < 0.15`, ngược lại `"OK"`.
```

**2. Bàn giao Slide / PowerPoint Template (Độ dài linh hoạt theo số lượng slide — ưu tiên dán vào Speaker Notes Slide 1 với bài nhiều slide):**
```markdown
# Bàn giao Slide: Báo cáo Biên lợi nhuận Q4 (Template)
- **Mục tiêu & Khán giả:** Báo cáo Ban Tài chính Q4 (`4 slide`, giữ nguyên layout/font chuẩn của template).
- **Nội dung & Tài nguyên cần chèn từng Slide:**
  - **Slide 1 (Trang bìa & KPI):** Thêm tiêu đề Q4 + tổng lợi nhuận ròng (`$5,237.5`) | `[Media: logo_cty.png]`
  - **Slide 2 (Hiệu suất khu vực):** Thêm 1 câu nhận xét APAC dẫn đầu | `[Chart: Biểu đồ cột từ Summary!A4:D8 (Doanh thu & Lợi nhuận theo Region)]`
  - **Slide 3 (Đơn cần rà soát):** Thêm nguyên nhân đơn biên `<15%` | `[Table: Bảng lọc từ Raw_Orders!A2:G500 với cờ "REVIEW"]`
  - **Slide 4 (Đề xuất hành động):** Thêm đề xuất trần chiết khấu `15%` | `[Screenshot: anh_cau_hinh_chiet_khau.png từ hệ thống QA]`
- **Bước tiếp theo:**
  1. Chèn biểu đồ cột vào Slide 2 và bảng đơn `"REVIEW"` vào Slide 3 theo đúng vùng nguồn trên.
  2. Dán ảnh chụp màn hình vào khung bên phải Slide 4 và hoàn thiện ghi chú trình bày.
```
