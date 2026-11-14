---
name: handoff
description: Sync multi-agent project state via HANDOFF.md or generate a copy-paste handoff block / file for Google Sheets, Excel (.xlsx), Google Slides, PowerPoint (.pptx), or another LLM (EN/VI). Use when starting or wrapping up work, syncing HANDOFF.md ("đồng bộ HANDOFF.md"), or when asked "handoff for sheet", "handoff for slides", "handoff for presentation", "viết hướng dẫn cho llm", "viết md hướng dẫn", "cho tôi hướng dẫn gg sheet cho AI", "cho tôi hướng dẫn slide cho AI", "tạo file hướng dẫn AI", "bàn giao cho sheet", "bàn giao qua Excel", or "bàn giao cho slide".
---
# Handoff Workflow
- **Start:** Read `HANDOFF.md` before touching any files.
- **End / LLM Guide File (`"viết hướng dẫn cho llm"` / `"viết md hướng dẫn"` / `"tạo file hướng dẫn AI"`):** Create or overwrite `HANDOFF.md` (`<20 lines`: Goal, Constraints, exact file paths, working/broken state, numbered next steps—no *"you're... doing..."* or *"bạn là... đang làm..."* fluff) and echo the `<20-line` Markdown block in chat (matching EN/VI).
- **Sheets / Excel Callout (`"handoff for sheet"` / `"cho tôi hướng dẫn gg sheet cho AI"` / `"bàn giao cho sheet"` / `"bàn giao qua Excel"`):** Output a standalone fenced `<20-line` Markdown block in chat (matching EN/VI) formatted to paste into `HANDOFF!A1` in Google Sheets or Excel (or write `.xlsx` if requested) with:
  1. **Context/Goal (`Mục tiêu`)** — for **reporting files**, explicitly include **Time Range & Update Frequency (`Kỳ báo cáo & Tần suất cập nhật`)** (e.g., `Q4 2026 (2026-10-01..2026-12-31) | Weekly every Monday`).
  2. **Data Lineage, Schema & Constraints (`Luồng dữ liệu, Cấu trúc & Ràng buộc`)** — for **reporting files**, explicitly state:
     - Which sheet gets data from which sheet (`<Sheet> ← <Source Sheet>`).
     - Which block gets data from where and using what method (`<Block Range> ← <Source Range> via <SUMIFS / XLOOKUP / QUERY / PivotTable / Python>`).
     - Protected columns/formulas not to overwrite.
  3. **Next Steps (`Bước tiếp theo`)** — numbered, exact cell/tab tasks.
- **Slides / Presentation Callout (`"handoff for slides"` / `"handoff for presentation"` / `"cho tôi hướng dẫn slide cho AI"` / `"bàn giao cho slide"`):** Output a standalone fenced `<20-line` Markdown block in chat (matching EN/VI) with: (1) Goal & Audience (`Mục tiêu & Khán giả`), (2) Verified Metrics & Slide Outline (`Số liệu chính & Bố cục Slide`), and (3) Next Steps (`Bước tiếp theo`)—formatted to paste into a hidden/skipped `HANDOFF` slide or Slide 1 Speaker Notes in Google Slides or PowerPoint (or generate `.pptx` if requested).
