# Known gaps

Parked issues in the current build, not fixed because they're out of scope for
v1 or because the right fix depends on information not yet available. Re-assess
after the first real prospecting session — does any of these actually cost
something, or is it still fine to leave parked.

## Shipped this batch (Fix Batch 02, 2026-07-30)

- **AUTO-HEADCOUNT — SHIPPED** (was parked above as of Fix Batch 01;
  post-freeze build 01, owner-authorised on evidence). `resurgam-ch-proxy`'s
  `/headcount/{companyNumber}` route: filing-history lookup for the latest
  accounts filing → document-metadata → document-content (iXBRL) →
  server-side regex extraction of `AverageNumberEmployeesDuringPeriod` (and
  taxonomy-prefix variants) → `{employees, period, accountsType}`, cached in
  KV per company + made-up-to date. On prospect selection (never during list
  render), the frontend auto-fills the Headcount field with provenance "N
  employees, FY[year] accounts (Companies House)"; owner overtype always
  wins and is what saves. Fail-soft coverage confirmed against real
  companies during build and again during Covenant-bridge testing: dormant
  accounts, micro-entity accounts with no employee note, and full accounts
  that simply omit the disclosure (Tesco Stores Ltd, 00519500, live-tested)
  all correctly fall through to the quick-pick range chips (1-9/10-49/
  50-249/250+, midpoint drives Pressure Cost) rather than erroring or
  silently showing nothing.

- **Covenant bridge — SHIPPED** (post-freeze build 02, owner-directed).
  Each Covenant-tab entry's primary action is now "Find at Companies
  House": a CH name search (suffix-cleaned via `_cleanCompanyName`) against
  `/ch-advanced?name=...` (the SIC-rich advanced-search endpoint, extended
  from its original SIC-only mode so the candidate list can show address +
  status + SIC together — the plain name-search endpoint used elsewhere
  never returns SIC). The owner's candidate pick is the confirmation step;
  it feeds into the same detail-panel machinery a Manual-tab CH lookup uses
  (`pickLookupResult`), then forces the Covenant tag, marks it confirmed,
  and carries the register's region/industry/pledge count onto the saved
  record. Dedupe: picking a CH company already in the pipeline surfaces a
  visible "already in pipeline" badge and updates that record in place
  (existing `saveCurrentProspect` merge-by-number logic) rather than
  duplicating. No match, or a non-company signatory (public bodies, trading
  names): "Add without lookup" falls back to the bare-add path, still
  Covenant-tagged and carrying the same region/industry/pledge metadata.
  Live-tested end to end against real GOV.UK Covenant entries: Tesco Stores
  Ltd (dedupe path exercised twice — second pick correctly flagged and
  merged, not duplicated) and Vanguard Training and Security Ltd (clean
  AUTO-HEADCOUNT success: "1 employees, FY2025 accounts").

  Found and fixed in passing: Companies House's advanced-search endpoint
  returns HTTP 404 (not 200 + empty items) when a name query genuinely
  matches nothing. `/ch-advanced` now treats 404 as zero results. **The
  same bug exists in `nova-proxy`'s identical `/ch-advanced` route** (this
  route was originally cloned from there) — per this repo's shared-core
  discipline that's normally a same-task fix in both, but nova-proxy is
  Nova's live production estate, so that fix needs your go-ahead before it
  lands there; flagging rather than pushing it silently.

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
