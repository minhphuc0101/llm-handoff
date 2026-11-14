---
name: handoff
description: Sync multi-agent project state via HANDOFF.md or generate a copy-paste handoff block for Google Sheets users. Use when starting or wrapping up work, syncing HANDOFF.md, or when asked for "handoff for sheet".
---
# Handoff Workflow
- **Start:** Read `HANDOFF.md` before touching any files.
- **End:** Overwrite `HANDOFF.md` (<20 lines, exact file paths, working/broken state, numbered next steps).
- **Google Sheets Callout (`"handoff for sheet"`):** Output a standalone fenced Markdown block in chat with: (1) Context/Goal, (2) Current Rules/Data Schema, and (3) Next Steps—formatted cleanly to paste into cell `A1` of a `HANDOFF` tab in Google Sheets.
