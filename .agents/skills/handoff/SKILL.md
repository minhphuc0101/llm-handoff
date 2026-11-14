---
name: handoff
description: Sync multi-agent project state via HANDOFF.md or generate a copy-paste handoff block / .xlsx workbook for Google Sheets and Excel users (EN/VI). Use when starting or wrapping up work, syncing HANDOFF.md ("đồng bộ HANDOFF.md"), or when asked for "handoff for sheet", "bàn giao cho sheet", "bàn giao qua Excel", or "xuất handoff".
---
# Handoff Workflow
- **Start:** Read `HANDOFF.md` before touching any files.
- **End:** Overwrite `HANDOFF.md` (`<20 lines`, exact file paths, working/broken state, numbered next steps—no persona fluff).
- **Sheets / Excel Callout (`"handoff for sheet"` / `"bàn giao cho sheet"` / `"bàn giao qua Excel"`):** Output a standalone fenced `<20-line` Markdown block in chat (in the user's language: English or Vietnamese) with: (1) Context/Goal (`Mục tiêu`), (2) Current Rules/Data Schema (`Cấu trúc & Quy tắc`), and (3) Next Steps (`Bước tiếp theo`)—formatted cleanly to paste into cell `A1` of a `HANDOFF` tab in Google Sheets or Excel (or write directly to `.xlsx` if requested).
