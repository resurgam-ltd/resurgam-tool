# Known gaps

Parked issues in the current build, not fixed because they're out of scope for
v1 or because the right fix depends on information not yet available. Re-assess
after the first real prospecting session — does any of these actually cost
something, or is it still fine to leave parked.

- **Places lookup / Find Trading Address / static maps return errors.**
  `resurgam-ch-proxy` is deliberately stripped to Companies House only (per the
  build brief); it never proxied Google Places, geocoding, or static maps.
  The frontend still has the UI for these (inherited from the fork, not
  removed since only Solar/Footprints/Visualiser/branding were named for
  removal) and will show "lookup failed" / HTTP 405 for them. Impact: no
  auto-fill for phone/website/site address or a map thumbnail; contact details
  must be entered manually. Low impact for v1 given headcount/sector/tag are
  the qualification fields that matter, but worth a decision if manual entry
  proves to be real outreach friction.

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
