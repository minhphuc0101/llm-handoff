---
name: handoff
description: Sync multi-agent project state via HANDOFF.md or generate a copy-paste handoff block / .xlsx workbook for Google Sheets, Excel, or another LLM (EN/VI). Use when starting or wrapping up work, syncing HANDOFF.md ("đồng bộ HANDOFF.md"), or when asked "handoff for sheet", "viết hướng dẫn cho llm", "viết md hướng dẫn", "cho tôi hướng dẫn gg sheet cho AI", "tạo file hướng dẫn AI", "bàn giao cho sheet", or "bàn giao qua Excel".
---
# Handoff Workflow
- **Start:** Read `HANDOFF.md` before touching any files.
- **End / LLM Guide File (`"viết hướng dẫn cho llm"` / `"viết md hướng dẫn"` / `"tạo file hướng dẫn AI"`):** Create or overwrite `HANDOFF.md` (`<20 lines`, exact file paths, working/broken state, numbered next steps—no *"you're... doing..."* or *"bạn là... đang làm..."* fluff) and echo the `<20-line` Markdown block in chat (matching EN/VI).
- **Sheets / Excel Callout (`"handoff for sheet"` / `"cho tôi hướng dẫn gg sheet cho AI"` / `"bàn giao cho sheet"` / `"bàn giao qua Excel"`):** Output a standalone fenced `<20-line` Markdown block in chat (matching EN/VI) with: (1) Context/Goal (`Mục tiêu`), (2) Current Rules/Data Schema (`Cấu trúc & Quy tắc`), and (3) Next Steps (`Bước tiếp theo`)—formatted to paste into `HANDOFF!A1` in Google Sheets or Excel (or write `.xlsx` if requested).
