---
title: Calisthenics Codex
date: 2026-11-17 09:00:00 -0500
subtitle: Personal Project
description: Calisthenics Codex - a natural-language workout log for calisthenics athletes and their coaches
image: '/images/portfolio/calisthenics-codex.jpg'
---

<div class="block-header inner-sm" style="margin-top: 1.5em; margin-bottom: 1.5em">
  <h2 class="block-title line-top">Overview</h2>
</div>

Calisthenics Codex is a workout log for calisthenics athletes and their coaches. Instead of tapping through dropdowns after every set, you type your session the way you'd text it, like "pullups 8/8/6, 3x10 dips, l-sit 30s", and it becomes structured sets you can review before saving. Athletes can share their training history with a coach, who gets a read-only view with progress charts.

As an online calisthenics coach, I built it around the way athletes already describe their training, and around what a coach actually needs to see. The app is live at [calisthenics-codex.vercel.app](https://calisthenics-codex.vercel.app), and the code is on [GitHub](https://github.com/atom-morgan/calisthenics-codex).

<div class="block-header inner-sm" style="margin-top: 1.5em; margin-bottom: 1.5em">
  <h2 class="block-title line-top">Contributions</h2>
</div>

I designed and built the whole application over four days, pairing with Claude Code. Each day started with a written plan that I reviewed before any code was written, every change was read before it was committed, and decisions and lessons were logged as they happened rather than reconstructed afterward.

The core feature is natural-language entry. Free text is sent to Claude server-side and comes back as structured sets, handling shorthand like 3x8 and 8/8/6, ladders and rounds, timed holds, added weight, and kg conversion. The parsed result fills the normal workout form, so nothing is saved until the user has reviewed it, and anything the parser couldn't read is shown as a warning instead of guessed. I picked Claude Haiku for parsing by running an eval against a fixed set of inputs: it matched Opus on accuracy while being faster and roughly a fortieth of the cost.

LLM output is never trusted on its own. Structured output fixes the response's shape, every set is then validated with the same Zod schema used by the manual form, and the final save is validated again like any other input.

For the coaching side, athletes add a coach by email and can remove them at any time, and coaches only see athletes who added them. Authorization lives in a data access layer next to the database queries rather than in the UI, and denied access returns a 404 so record ids can't be probed. Supabase's REST API is locked down so the app's own server is the only way in.

Database tests run against PGlite, an in-memory Postgres, using the real migrations, so constraints, triggers, and access rules run as real SQL with no Docker or secrets needed in CI. The test suite includes the authorization rules themselves, written so they actually fail if an access check is removed. GitHub Actions runs formatting, linting, type checks, tests, and a production build on every push.

<div class="block-header inner-sm" style="margin-bottom: 1.5em">
  <h2 class="block-title line-top">Technology</h2>
</div>

Some of the primary technologies used for this project are listed below:

- Next.js 16 (App Router, Server Components, Server Actions)
- React 19
- TypeScript
- Tailwind CSS
- Postgres on Supabase with Drizzle ORM
- Supabase Auth
- Zod
- Anthropic API (Claude Haiku)
- Recharts
- Vitest with PGlite
- GitHub Actions
- Vercel
