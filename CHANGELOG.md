# Claude Preferences — Changelog
# Author: David Baker
# Purpose: Version history for preferences-current.md with diff-level detail.
# Use this to understand exactly what changed between versions and to roll back.
# All content © David Baker. Do not share or distribute.

---

## How to Roll Back

1. Open the version file you want to restore (e.g. `preferences-v1.1-20260428.md`)
2. Copy it to `preferences-current.md` — overwrite the current file
3. Push to GitHub:
   ```
   git add . && git commit -m "rollback to vX.X YYYYMMDD" && git push
   ```
4. Add a rollback entry to this CHANGELOG explaining why

---

## VERSION 1.2 — May 6, 2026
Deprecates: v1.1 (April 28, 2026)
File: preferences-current.md
Prior version archived as: preferences-v1.1-20260428.md

### What changed and why

**Added: Technical Builds — Pre-Flight Checklist (new section)**
- v1.1 behavior: No pre-flight check. Claude would proceed directly to build tasks without verifying environment readiness.
- v1.2 behavior: Before any platform build, Claude explicitly verifies: required Vercel env vars are present and named; required Supabase tables exist with correct grants; SQL setup files include GRANT statements; Claude Code scripts never use interactive input (getpass/TTY); .env file location is confirmed; direct URLs used for navigation guidance.
- Why: Auth build on May 6 wasted significant time due to missing SUPABASE_SECRET_KEY in Vercel, missing table grants, and a script that failed because Claude Code can't accept interactive terminal input.

**Added: LEARNINGS.md Protocol (new section under Packaging and Transfer)**
- v1.1 behavior: No LEARNINGS.md. Operational lessons were not captured in any structured way.
- v1.2 behavior: LEARNINGS.md is maintained as a living, project-agnostic document. Updated automatically at every transfer. Organized by system (Supabase, Vercel, Claude Code, etc.). README must reference it as required reading before any platform build. Loading a new thread now includes reading LEARNINGS.md as step 3.
- Why: Repeated friction with the same classes of errors (table permissions, env vars, TTY input) that would have been avoided with a reference document.

**Modified: Loading a New Thread (Packaging and Transfer section)**
- v1.1 behavior: Steps were: Read README → Read CHANGELOG → Read session notes → State workstreams → Ask where to start.
- v1.2 behavior: Added "Read LEARNINGS.md" as step 3, before session notes. Full sequence: Read README → Read CHANGELOG → Read LEARNINGS.md → Read session notes → State workstreams → Ask where to start.
- Why: LEARNINGS.md is only useful if it's actually consulted at session start, not just packaged.

**Modified: Technical Builds — General (Task Execution section)**
- v1.1 behavior: No guidance on handling Claude Code self-recovery or screenshot analysis.
- v1.2 behavior: Added two rules — (1) when Claude Code hits an error and self-recovers, note it but do not interrupt unless stuck or looping; (2) analyze screenshots of UIs to learn navigation before giving step-by-step instructions.
- Why: During auth build, Claude Code recovered from a Node.js error on its own and interrupting would have broken the flow. Also, imprecise navigation instructions (e.g. "Settings → API") sent the user to the wrong Supabase page multiple times.

**Modified: Terminal Instructions (Task Execution section)**
- v1.1 behavior: Said never to ask user to paste API keys or credentials into chat.
- v1.2 behavior: Strengthened to "Never ask the user to paste API keys, credentials, or sensitive values into chat — ever. Always say so explicitly." Also added: "Cmd+T opens a new Terminal tab separate from Claude Code."
- Why: The TTY/getpass failure during auth build meant the user needed a plain Terminal tab, and there was no guidance on how to get one without leaving Claude Code.

**Modified: Supabase notes (Tool and Platform Notes section)**
- v1.1 behavior: Noted 42501 errors require grants but didn't say to include grants proactively in SQL scripts.
- v1.2 behavior: Added "Always include GRANT statements in any SQL setup script — never assume defaults." Added service role key location as a direct path: supabase.com/dashboard/project/[project-id]/settings/api.
- Why: setup-supabase.sql was missing GRANT statements, causing a 42501 on the users table during auth build.

**Modified: Vercel notes (Tool and Platform Notes section)**
- v1.1 behavior: Basic redeploy instructions. No note about preview URL 403s or which key is needed for auth.
- v1.2 behavior: Added — 403 on preview URLs is normal, use the assigned domain. Auth against Supabase users table requires SUPABASE_SECRET_KEY (service_role), not just SUPABASE_ANON_KEY. Always provide direct URLs for navigation, not click-path descriptions.
- Why: User hit a confusing 403 on a preview URL and spent time troubleshooting a non-issue. Missing SUPABASE_SECRET_KEY caused repeated login failures.

**Modified: Baker Legal Search App notes (Tool and Platform Notes section)**
- v1.1 behavior: Listed app as "currently unprotected."
- v1.2 behavior: Updated to reflect auth is now live. Added auth details: users table, bcryptjs, HMAC session cookie, temporary password Baker2024! for both accounts.
- Why: Auth was completed May 6, 2026.

**Modified: Python / Mac notes (Tool and Platform Notes section)**
- v1.1 behavior: No note about Claude Code TTY limitations.
- v1.2 behavior: Added "Claude Code terminal does not support interactive input (getpass, TTY prompts) — always use hardcoded args or env vars."
- Why: setup-users.py used getpass and failed in Claude Code twice during auth build.

---

## VERSION 1.1 — April 28, 2026
Deprecates: DavidBaker-ClaudePreferences-20260428.md (v1.0)
File: preferences-v1.1-20260428.md

### What changed
- Initial structured preferences document replacing informal v1.0
- Established file naming convention (BakerCoParenting-[Description]-YYYYMMDD-HHMM)
- Established Claude Code as default tool for all Workstream B tasks
- Documented Supabase, Vercel, Git, Python platform notes
- Established CHANGELOG.md and session notes as transfer package requirements
- Defined Anna-facing communication rules including Justin confidentiality rule

---

*Last updated: May 6, 2026*
