# llm-handoff

Ultra-light (`<200 tokens`) multi-agent state handoff & context bridge for **Antigravity**, **Google Jules**, **Cursor**, **GitHub Copilot**, and **Gemini in Google Sheets**—pass project state back and forth without context loss or token bloat.

## Repository Structure

```text
llm-handoff/
├── AGENTS.md                           # Master contract (auto-read by Antigravity, Jules & Copilot)
├── HANDOFF.md                          # Live status baton (<20 lines)
├── .agents/
│   └── skills/
│       └── handoff/
│           └── SKILL.md                # Auto-discovered workspace skill for Antigravity
└── README.md                           # 1-line installer & team SOP
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
| **Antigravity** | Automatic | Auto-loads `AGENTS.md` & `.agents/skills/handoff/SKILL.md`; reads `HANDOFF.md` on start and updates on wrap-up. Type `handoff for sheet` to export for Google Sheets. |
| **Cursor / Copilot** | `@AGENTS.md` | Mention `@AGENTS.md` at session start/end (or auto-loaded in supported versions). |
| **Gemini in Google Sheets** | `HANDOFF!A1` | Paste the exported Markdown block into cell `A1` of a `HANDOFF` tab, then prompt: *"Read the HANDOFF tab first, then complete Next Step 1."* |

## Team Workflow Summary (Repo ↔ Google Sheets)

1. **Code → Sheet (`handoff for sheet`):**
   - Developer finishes coding in Antigravity/Cursor and types `handoff for sheet`.
   - Agent updates `HANDOFF.md` in Git and outputs a `<20-line` self-contained Markdown summary in chat.
   - Developer copies that block and pastes it into cell `A1` of a tab named `HANDOFF` in the shared Google Sheet.

2. **Working inside Google Sheets:**
   - Teammate opens Gemini in the Sheets side panel and prompts:
     > *"Read the HANDOFF tab first to understand the project rules and context, then help me complete Next Steps 1–3."*

3. **Sheet → Code (`Sync HANDOFF.md`):**
   - When finished in Sheets, teammate asks Gemini in Sheets:
     > *"Update the text in HANDOFF!A1 with what we just completed and the next steps for the developer."*
   - Teammate sends that block back to the developer, who pastes it into Antigravity with: `Sync HANDOFF.md with this update.`
