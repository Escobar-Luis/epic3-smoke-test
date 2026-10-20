# Memlog

- Decision: reuse the template's one route and `submissions` table; the only new behavior is the system prompt. Source: brief/problem.md ("One form, one API route, one table").
- Decision: summary format is exactly three "- " bullets in plain English, from the brief ("three-bullet plain-English summary").
- Decision: the 3 recorded runs were written by Claude Sonnet 5.5 (`claude-sonnet-5-5`), the model running this build, answering the same system prompt; the example changelogs are made up for the demo, not a real company's. They were not produced by the Workers AI/Groq/OpenRouter chain, because no provider keys exist in the build sandbox, so `model` says `claude-sonnet-5-5`.
- Open question: the brief names no real Startup; "Epic 3 Smoke Test" (smoke-test.invalid) is used as given in project.json. No evidence beyond the template repo link.
- Open question: live output quality of the free models against the system prompt was not measured here.
