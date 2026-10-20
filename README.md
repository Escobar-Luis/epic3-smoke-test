# startup-project-template

The template every Project starts from: a live, $0, full-stack demo with its own database, a stable link and a demo mode.
`gh repo create <name> --public --template Escobar-Luis/startup-project-template`.

## Problem

Release notes are written for engineers. A customer who wants to know what changed has to read a raw changelog. This Project turns a pasted changelog into three plain-English bullets. It is a smoke test of the template, not a real company ("Epic 3 Smoke Test").

## Evidence

- https://github.com/Escobar-Luis/startup-project-template: the template and stack this smoke test exercises (the only source in `brief/`).

## What was built

1. A page where a visitor pastes a changelog (max 2000 characters) and gets a three-bullet customer summary.
2. `POST /api/ask` sends the text through `llm()` with a fixed system prompt (`SUMMARY_SYSTEM` in `src/worker/index.ts`): Workers AI, then Groq, then OpenRouter, then a recorded run with a banner.
3. Each submission is stored in the `submissions` table under row-level security.
4. Three recorded runs in `demo/runs.json`, written by `claude-sonnet-5-5` on made-up changelogs. The live free models were not run during the build, and their output quality is untested.
5. An architecture explainer at `/explainer.html`, and the GitHub Pages front from the template.

The footer reads "independent demo by Luis Escobar, not affiliated with Epic 3 Smoke Test".

## How to run

```bash
npm ci
npm test          # vitest; providers and the database are mocked
npm run build     # explainer, type-check, Vite build to dist/, and a check that no secret reached dist/
npm run explainer # only rebuild public/explainer.html from public/explainer.json
npm run build:worker   # wrangler dry-run bundle of the Worker
```

Deploys run only in GitHub Actions (`ship.yml`), never on a laptop. One-time setup per Project:

```bash
gh secret set CLAUDE_CODE_OAUTH_TOKEN --env build
gh secret set CLOUDFLARE_API_TOKEN --env deploy     # Workers Scripts:Edit
gh secret set CLOUDFLARE_ACCOUNT_ID --env deploy
gh secret set NEON_API_KEY --env deploy
gh secret set GROQ_API_KEY --env deploy             # optional
gh secret set OPENROUTER_API_KEY --env deploy       # optional
gh workflow run ship.yml -f max_turns=80 -f timeout_minutes=60
```

Pages is served from the `gh-pages` branch, which the deploy job writes. The workflow's token cannot turn Pages on,
so the deploy fails with "Pages not enabled" until it is on. The build step (story 3.9) enables it once from the Mac:

```bash
git push origin "$(git commit-tree "$(git hash-object -t tree /dev/null)" -m 'chore: gh-pages')":refs/heads/gh-pages
gh api -X POST repos/Escobar-Luis/<repo>/pages -f build_type=legacy -f 'source[branch]=gh-pages' -f 'source[path]=/'
```

## stack_note

None: this Project uses the template stack in `STACK.md` unchanged.
