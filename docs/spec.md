# Spec

A visitor pastes a product changelog (up to 2000 characters) and gets three plain-English bullets for customers.

Story 1: `/api/ask` sends a fixed system prompt (`SUMMARY_SYSTEM`) with the pasted text through `llm()`. Test in `tests/app.test.ts`.
Story 2: the page says "Paste a changelog" / "Summarize"; recorded examples are changelogs.
Story 3: `demo/runs.json` holds 3 recorded runs, served in demo mode.
Story 4: README, explainer, review.

No new table: `submissions` (with RLS) already stores each input and output.
