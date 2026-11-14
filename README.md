# llm-handoff

Ultra-light (`<200 tokens`) multi-agent state handoff & context bridge for **Antigravity**, **Google Jules**, **Cursor**, **GitHub Copilot**, **Gemini in Google Sheets & Slides**, **Excel Copilot**, and **PowerPoint Copilot**—pass project state back and forth without context loss, roleplay fluff, or token bloat (English & Tiếng Việt).

## Repository Structure

```text
llm-handoff/
├── AGENTS.md                           # Master contract (auto-read by Antigravity, Jules & Copilot)
├── HANDOFF.md                          # Live status baton (<20 lines)
├── .agents/
│   └── skills/
│       └── handoff/
│           └── SKILL.md                # Auto-discovered workspace skill for Antigravity (EN/VI)
└── README.md                           # 1-line installer & bilingual team SOP
```

## 1-Line Install (Run in any project root)

**Bash / macOS / Linux / Git Bash:**
```bash
mkdir -p .agents/skills/handoff && curl -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/AGENTS.md -o AGENTS.md && curl -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/HANDOFF.md -o HANDOFF.md && curl -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/.agents/skills/handoff/SKILL.md -o .agents/skills/handoff/SKILL.md
```

**PowerShell (Windows):**
```powershell
New-Item -ItemType Directory -Force .agents/skills/handoff | Out-Null; curl.exe -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/AGENTS.md -o AGENTS.md; curl.exe -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/HANDOFF.md -o HANDOFF.md; curl.exe -sL https://raw.githubusercontent.com/minhphuc0101/llm-handoff/main/.agents/skills/handoff/SKILL.md -o .agents/skills/handoff/SKILL.md
```

## Team Callout Cheat Sheet

| Tool | Trigger | Action |
| :--- | :--- | :--- |
| **Google Jules** | Automatic | Reads `AGENTS.md` on every task/issue and updates `HANDOFF.md` in its PR. |
| **Antigravity** | Automatic | Auto-loads `AGENTS.md` & `.agents/skills/handoff/SKILL.md`; reads `HANDOFF.md` on start and updates on wrap-up. Type `handoff for sheet`, `handoff for slides`, `cho tôi hướng dẫn gg sheet cho AI`, `cho tôi hướng dẫn slide cho AI`, or `viết hướng dẫn cho llm`. |
| **Cursor / Copilot** | `@AGENTS.md` | Mention `@AGENTS.md` at session start/end (or auto-loaded in supported versions). |
| **Gemini in Google Sheets / Excel Copilot** | `HANDOFF!A1` | Paste the exported Markdown block into cell `A1` of a `HANDOFF` tab (or open the generated `.xlsx`), then prompt: *"Read the HANDOFF tab first, then complete Next Step 1."* |
| **Gemini in Google Slides / PowerPoint Copilot** | Hidden `HANDOFF` slide / Slide 1 Notes | Paste the exported Markdown block into a skipped/hidden `HANDOFF` slide or Slide 1 Speaker Notes (or open the generated `.pptx`), then prompt: *"Read the HANDOFF slide / Slide 1 notes first, then create/update Slides 2–5."* |

## Team Workflow Summary (Repo ↔ Sheets/Excel ↔ Slides/PPTX)

1. **Code → Sheet/Excel (`handoff for sheet`):**
   - Developer finishes coding in Antigravity/Cursor and types `handoff for sheet` (or asks the agent to write an `.xlsx` workbook with a `HANDOFF` tab).
   - Agent updates `HANDOFF.md` in Git and outputs a `<20-line` self-contained Markdown summary in chat.
   - Developer copies that block and pastes it into cell `A1` of a tab named `HANDOFF` in Google Sheets or Excel.

2. **Working inside Google Sheets / Excel:**
   - Teammate opens Gemini in Google Sheets (or Excel Copilot) and prompts:
     > *"Read the HANDOFF tab first to understand the project rules and context, then help me complete Next Steps 1–3."*

3. **Sheet/Excel → Code (`Sync HANDOFF.md`):**
   - When finished in Sheets/Excel, teammate asks the spreadsheet AI:
     > *"Update the text in HANDOFF!A1 with what we just completed and the next steps for the developer."*
   - Teammate sends that block back to the developer, who pastes it into Antigravity with: `Sync HANDOFF.md with this update.`

4. **Code/Sheet → Presentation (`handoff for slides` / `handoff for presentation`):**
   - Developer types `handoff for slides` (or asks the agent to generate a `.pptx` with a hidden `HANDOFF` slide) and pastes the `<20-line` block into a skipped/hidden `HANDOFF` slide or Slide 1 Speaker Notes.
   - Teammate opens Gemini in Google Slides (or PowerPoint Copilot) and prompts:
     > *"Read the HANDOFF slide / Slide 1 notes first, then create or update Slides 2–5."*

---

## Tiếng Việt — Mô tả & Câu lệnh Bàn giao

### Mô tả dự án
`llm-handoff` là giao thức bàn giao trạng thái siêu nhẹ (`<200 tokens`) giúp các AI coding agent (**Antigravity**, **Google Jules**, **Cursor**, **GitHub Copilot**), AI bảng tính (**Gemini trong Google Sheets**, **Copilot trong Excel**) và AI thuyết trình (**Gemini trong Google Slides**, **Copilot trong PowerPoint `.pptx`**) chia sẻ chung một nguồn sự thật qua `HANDOFF.md`, `HANDOFF!A1` và slide `HANDOFF` ẩn. Thay vì phải viết prompt dài kiểu *"Bạn là lập trình viên... hiện tại bạn đang làm..."*, nhóm chỉ cần dùng trạng thái khai báo rõ ràng: **Mục tiêu**, **Phần đã chạy**, **Lỗi/WIP**, và **Bước tiếp theo**.

### Bảng câu lệnh Tiếng Việt (Callouts)

| Giai đoạn | Nơi gõ lệnh | Câu lệnh mẫu (Tiếng Việt) |
| :--- | :--- | :--- |
| **Cài đặt vào dự án** | Antigravity / Cursor | `"Cài đặt https://github.com/minhphuc0101/llm-handoff vào dự án này"` |
| **Tạo / cập nhật `HANDOFF.md` cho LLM khác** | Antigravity / Cursor | `"viết hướng dẫn cho llm"` hoặc `"viết md hướng dẫn"` hoặc `"tạo file hướng dẫn AI"` |
| **Xuất khối Markdown (`Code → Sheet/Excel`)** | Antigravity / Cursor | `"cho tôi hướng dẫn gg sheet cho AI"` (hoặc `"bàn giao cho sheet"` / `"bàn giao qua Excel"`) |
| **Xuất trực tiếp file `.xlsx`** | Antigravity / Cursor | `"Cập nhật HANDOFF.md và tạo file Excel bàn giao có tab HANDOFF ở ô A1"` |
| **Xuất khối Markdown cho Slide (`Code/Sheet → Slide/PPTX`)** | Antigravity / Cursor | `"cho tôi hướng dẫn slide cho AI"` (hoặc `"bàn giao cho slide"` / `"handoff for slides"`) |
| **Xuất trực tiếp file `.pptx`** | Antigravity / Cursor | `"Cập nhật HANDOFF.md và tạo file PowerPoint (.pptx) có slide HANDOFF ẩn ở đầu"` |
| **Làm việc trong Sheet / Excel** | Gemini (Sheets) / Copilot (Excel) | `"Đọc tab HANDOFF trước để nắm mục tiêu và quy tắc, sau đó thực hiện Bước tiếp theo 1–3."` |
| **Làm việc trong Google Slides / PowerPoint** | Gemini (Slides) / Copilot (PPTX) | `"Đọc slide HANDOFF (hoặc ghi chú Slide 1) trước để nắm mục tiêu, số liệu và bố cục, sau đó hoàn thiện các slide tiếp theo."` |
| **Cập nhật khi làm xong (`Sheet/Slide → Code`)** | Sheets / Excel / Slides / PPTX | `"Cập nhật lại ô HANDOFF!A1 (hoặc slide HANDOFF) với những việc vừa hoàn thành và bước tiếp theo cho dev."` |
| **Đồng bộ lại vào Code** | Antigravity / Cursor | `"Đồng bộ HANDOFF.md theo nội dung này: <dán nội dung HANDOFF>"` |

### Ví dụ khối bàn giao Tiếng Việt (`<20 dòng`, không cần *"Bạn là... đang làm..."*)

**1. Bàn giao sang Sheet / Excel (`HANDOFF!A1`):**
```markdown
# Bàn giao: Rà soát biên lợi nhuận Q4
## 1. Mục tiêu
- Tính biên lợi nhuận ròng đơn hàng Q4 ở tab `Orders` và gắn cờ đơn có biên <15%.

## 2. Cấu trúc & Quy tắc (`Orders`, dòng 2-6)
- Cột `A..E`: `Order_ID`, `Region`, `Gross_USD`, `Discount_Pct`, `Shipping_USD`
- `Net_Margin_USD` (F): `Gross_USD * (1 - Discount_Pct) - Shipping_USD`
- `Low_Margin_Flag` (G): `"REVIEW"` nếu `Net_Margin_USD / Gross_USD < 0.15`, ngược lại `"OK"`

## 3. Bước tiếp theo
1. Điền công thức `Net_Margin_USD` vào `Orders!F2:F6`.
2. Điền công thức gắn cờ `Low_Margin_Flag` vào `Orders!G2:G6`.
3. Tạo tab `Summary` tổng hợp `Net_Margin_USD` theo từng `Region`.
```

**2. Bàn giao sang Slide / PowerPoint (Slide `HANDOFF` ẩn hoặc Speaker Notes Slide 1):**
```markdown
# Bàn giao Slide: Báo cáo Biên lợi nhuận Q4
## 1. Mục tiêu & Khán giả
- Trình bày kết quả biên lợi nhuận ròng Q4 và danh sách đơn cần rà soát cho Ban Tài chính (5 slide, ngắn gọn).

## 2. Số liệu chuẩn & Bố cục Slide
- Nguồn chuẩn: Tab `Summary` trong `handoff_workbook.xlsx` (NA: $1,285 | EMEA: $1,767.5 | APAC: $2,185).
- Slide 1: Tiêu đề & Tóm tắt điều hành | Slide 2: Biên lợi nhuận theo khu vực | Slide 3: Đơn hàng gắn cờ `REVIEW` (<15%) | Slide 4: Đề xuất tối ưu chiết khấu & vận chuyển.

## 3. Bước tiếp theo
1. Tạo Slide 2 với biểu đồ cột so sánh `Net_Margin_USD` theo `Region`.
2. Đưa bảng các đơn `Low_Margin_Flag = "REVIEW"` vào Slide 3 và đề xuất hướng xử lý ở Slide 4.
```
