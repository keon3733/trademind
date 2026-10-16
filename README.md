# TradeMind

Master your behavior. Improve your trading.

A trading performance and behavioral-intelligence web app for retail forex traders (data model open to futures and other markets). It is a **trading-improvement tool** — not a signal service, and it never guarantees profits or places broker orders.

## Run it

```bash
npm install
npm run dev        # http://localhost:5173
npm test           # 98 tests: metrics, CSV import, duplicates, alerts, access control (RLS)
npm run build      # typecheck + production build
```

**Demo mode** (default, no setup): click "Continue in demo mode" on the sign-in page. Data lives in this browser only. Use "Load sample data" on the empty dashboard to explore — sample trades are labelled manual/unverified.

## Enable accounts and sync (Supabase)

1. Create a Supabase project.
2. Run `supabase/migrations/0001_init.sql` in the SQL editor (tables, row-level security on every private table, `delete_my_account()`).
3. Copy `.env.example` to `.env.local` and set `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` (public anon key only).
4. Restart `npm run dev`. Sign-up/sign-in, per-user isolation, export and account deletion now use Supabase.

## Documentation (in `docs/`)

- `TUTORIAL.md` — full written walkthrough: zip → local run → Supabase → website → installable phone app → daily use
- `PUBLIC_LAUNCH_CHECKLIST.md` — security, legal and operational gates before inviting the public
- `TERMS_OF_USE.md` — rules and regulations for using the website (template; lawyer review needed)
- `PRIVACY_POLICY.md` — privacy policy template

Hosting files included: `vercel.json`, `netlify.toml`, `public/_redirects` (page-refresh fix and security headers). Installable web app files: `public/manifest.webmanifest`, `public/sw.js`, icons.

## What's inside

| Area | Where |
|---|---|
| Dashboard, filters, equity curve, discipline breakdown | `pages/Dashboard.tsx` |
| Journal: manual entry, tags, self-reported emotion, provenance badges | `pages/Journal.tsx` |
| CSV import: column mapping, preview, row fixes, duplicate detection, audit copy | `pages/ImportPage.tsx`, `lib/csv.ts` |
| Performance by instrument/setup/session/weekday/risk size, sample-size warnings | `pages/Performance.tsx`, `lib/metrics.ts` |
| Behavior alerts (13 explainable rules) with confirm / correct / dismiss | `pages/Behavior.tsx`, `lib/behavior.ts` |
| Rule-following lab | `pages/Behavior.tsx`, `lib/lab.ts` |
| Pre-trade checkpoint, session protection, voluntary lock, override log | `pages/Coach.tsx`, `lib/protection.ts` |
| Post-session review | `pages/Reports.tsx`, `lib/review.ts` |
| Terms of Use and Privacy Policy pages (`/terms`, `/privacy`), sign-up consent | `pages/Legal.tsx`, `lib/legal.ts`, `lib/markdown.ts`, `docs/` |
| Plan, risk limits, checklist, score weights, change log; export; coach consent; delete account | `pages/Settings.tsx` |

All calculations and rules are deterministic, framework-free modules in `src/lib` with unit tests. No AI is used.

## Honest limits

- Session protection works **inside TradeMind only**. It warns, asks for a reason and records what happened; it cannot stop orders at a broker.
- The discipline score uses provisional, configurable weights and is **not scientifically validated**.
- Alerts are possibilities with evidence, never diagnoses. Emotional states come only from the trader's own self-reports, never from P&L.
- Metrics describe past trades and do not predict future results.
- Session boundaries (Asia/London/New York) are a simple UTC-hour convention used only when a trade has no session set.
