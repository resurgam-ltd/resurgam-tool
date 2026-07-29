# Known gaps

Parked issues in the current build, not fixed because they're out of scope for
v1 or because the right fix depends on information not yet available. Re-assess
after the first real prospecting session — does any of these actually cost
something, or is it still fine to leave parked.

## Top of post-freeze candidate list

- **AUTO-HEADCOUNT** (spec recorded 2026-07-29, build later on evidence from a
  real prospecting session — NOT built now, feature freeze is in effect).
  New Worker route on `resurgam-ch-proxy` hitting the Companies House
  Documents API, fetching the latest filed accounts' iXBRL document, parsing
  the average-employees disclosure out of it, and populating both the
  Headcount field and the Pressure Cost panel automatically — no manual step
  between a search result and generated proposal figures. Expected payoff:
  removes the last manual step in the qualify workflow. Prerequisite
  research before building: confirm the Documents API's auth model (likely
  the same CH_KEY Basic auth already used elsewhere), iXBRL's actual
  disclosure tag for average employees (not all filers use the same
  taxonomy — small-company filers under FRS 105 may omit it entirely,
  dormant/micro-entity accounts often don't file it at all), and a fail-soft
  path for the (common) case where the disclosure isn't present, mirroring
  the Covenant list's cache-and-fail-soft pattern rather than erroring per
  prospect.

## Other parked items

- ~~Places lookup / Find Trading Address / static maps return errors.~~
  **RESOLVED by removal (Change Order 03).** The whole Locate/Measure
  workflow (Search Company via Google Places, Find Trading Address, Google
  Earth KML export, site-area capture) was Nova's roofing-survey worldview
  and has been deleted outright, not left broken and deferred. Reach is now
  manual-only by design (PECR — contacts found manually, no scraping/lookup
  services), which was the intended contact-sourcing model anyway.

- **No Resurgam Airtable base yet.** `AT_BASE`/`AT_COMPANIES`/`AT_SITES` are
  deliberately empty; all saves are localStorage-only (see CLAUDE.md). This
  means: single-browser, single-device pipeline (no shared team view), no
  server-side backup of saved prospects beyond CSV export, and the legacy
  Airtable-shaped sync code paths (patch-on-name-edit, cache polling) are
  dormant, not deleted. Impact: fine for one person building the pipeline
  solo; becomes a real gap the moment a second person needs to see the same
  list, or a browser's storage gets cleared without a recent CSV export.

- **Parked SIC candidates**, noted in-code (`SECTOR_SICS` TODO comment):
  71111 (architects), 73110 (advertising agencies) for professional_services;
  62011 (games development) for tech_software; marine-services codes (not yet
  specified) for defence_marine. Not built pending your review of the
  mapping-review ruling.

- **Email variant copy is DRAFT placeholder**, not real copy. Labels, framing
  and door-tag defaults are final; the actual subject/body text for
  Owner/People/Covenant variants still needs to come from the business
  documents.

- **Pressure Cost panel uses one GB-wide rate for all six sectors.** HSE does
  not publish stress/depression/anxiety prevalence broken down finely enough
  for a defensible per-sector multiplier, so all sectors currently share the
  same national average (see the panel's own disclaimer). If HSE or another
  credible source publishes sector-level rates that actually map to these six
  groups, that would be a real accuracy improvement, not just a nice-to-have.

- **"ERS" read as Employer Recognition Scheme** (the Armed Forces Covenant's
  companion scheme) — a reasonable inference given context, not something you
  confirmed explicitly. If that's wrong, the CSV-import defaults (Covenant
  door tag, defence_marine sector) for ERS-sourced rows need revisiting.

- **Covenant list caching — KV usage confirmed within Cloudflare's free tier.**
  Per Cloudflare's current published limits (100,000 KV reads/day, 1,000
  writes/day, 1GB storage): a rebuild costs 10 upstream requests to GOV.UK's
  Search API (14,738 records ÷ 1,500 per request, rounded up) plus 2 KV
  writes (data + asOf); a cache-hit request costs 2 KV reads. The stored list
  is ~2-3MB, well under both the 1GB total and the 25MB per-value limit. At
  one rebuild per ~24h this uses a small fraction of the daily write/read
  budget — would need roughly 500x today's traffic before free-tier limits
  became a real constraint.

- **Covenant match discipline — containment heuristic can flag unrelated
  near-matches** (e.g. "Capita Plc" flagged as a possible match against
  "...Grosvenor Capital Limited", because "capital" contains "capita" as a
  substring). Confirmed safe by design — this only produces an extra
  confirm-prompt the owner correctly rejects, never a wrong assertion. TODO,
  parked, do not build now: tighten the "possible" match to word-boundary
  token comparison instead of raw substring containment, to cut confirm
  noise. Low priority — re-assess only if false "possible" prompts turn out
  to be a real annoyance in practice, not on principle.

---

**FEATURE FREEZE in effect (Change Order 03 review, 2026-07-27):** resurgam-tool
v1 is feature-complete. Only bugfixes and the three real email variant
copy-ins (arriving from the planning channel with the business documents)
land from here until at least one real prospecting session has produced
evidence about what actually earns a build. Don't add to this list's items
speculatively — new gaps get added when the first real session actually hits
them, not before.
