--- TIERED AUTONOMY (2026-07-30, supersedes prior gate lists) ---
Principle: the brief from the planning channel carries the safety. The
job is to execute it, not to seek reassurance. Jamie is stopped only for
things that can materially cost him.

TIER 1, PROCEED SILENTLY. Everything reversible, local, or invisible
outside the estate: reads, edits, commits, branches, local builds, dry
runs, pushes to the unannounced Resurgam repos, research, verification.
No prompts, no confirmations. Log it and move on.

TIER 2, PROCEED AND REPORT. Consequential but explicitly covered by the
brief: proceed, then state plainly in the report what was done and what
it affected. If the brief authorised it, that authorisation is not
re-asked.

TIER 3, STOP AND CONFIRM TWICE. Only these, and only when the brief does
not already authorise the specific action:
   a) Money: any spend or new paid service.
   b) Irreversible destruction: deleting data, history or backups.
      Destructive git actions remain hard-blocked and require deliberate
      unblocking.
   c) Credentials: creating, placing or changing any secret. Jamie
      places secrets himself.
   d) Reaching a third party: sending, publishing, announcing,
      submitting.
   e) Live estates: real changes to Nova's running systems.
   f) Personal data moving somewhere new.
   g) Scope exit that is also irreversible.
Double confirmation means: first a plain-English gate stating what
happens, why now, cost if wrong, and the recommendation; then,
immediately before executing, one short restatement and a final
go-ahead. The second ask exists so nothing irreversible happens on a
single reflex click.

THE ALARM TEST, apply before any stop. Ask: is it reversible, does
anyone outside see it, does it cost money, can it be undone in one step?
If reversible and unseen, it is a lit match: proceed and log it. Only
raise the alarm for a real fire. A stop that turns out to be Tier 1 is
itself a defect; report it as one so the rule can be tightened.

UNCERTAINTY. If unsure and a reversible path exists, take the reversible
path and report. Only stop when the uncertainty is itself irreversible.

NO MID-FLOW PROMPTS. Batch anything needing Jamie into one end-of-phase
report. Never per file, never per command.

PLAIN ENGLISH. Every message to Jamie is answerable by a non-coder from
the text alone. REWRITE THIS GATE remains his standing bounce, and it
holds work safely rather than cancelling it.
--- end ---

# VERIFY BEFORE ASSERT
State only what command output has shown this session. Prove claims that matter
before making them.

VERIFICATION DOCTRINE UPGRADE (2026-07-27): HTTP status alone is never
verification. A live check passes only when the served content contains an
expected sentinel unique to the artefact. Standard sentinel: a build stamp
(short commit hash + build date) embedded in the page, in a meta tag and in
the visible footer. A live check fetches the served page and matches the
stamp against current HEAD. Originated here: every prior "200 OK" check
against this repo's Pages URL was unknowingly hitting an auto-generated
Jekyll README placeholder (no index.html existed), not the deployed tool —
now fixed, and the stamp below is the safeguard against it recurring.
SELF-REFERENCE NOTE, corrected (Change Order 03 review): the served build
stamp MUST equal HEAD's parent — not "HEAD or HEAD's parent". A commit
cannot embed its own hash, so a stamp-bump commit's stamp always names its
parent; the stamp-bump commit is therefore always current HEAD itself, by
construction, every time. Any stamp older than HEAD's parent fails the check
outright — that means a stale or partial deploy, not an acceptable variant.
Never loosen this to "HEAD or an ancestor" — that would let a stale deploy
pass a sentinel check meant to catch exactly that.

LIVE-DEPLOY VERIFICATION DOCTRINE (2026-07-30): a deploy to an
edge-distributed service is verified only after propagation. Allow a settle
window, then verify with repeated, spaced requests covering both a
known-good case and a known-boundary case, requiring consistency across the
full sample. A single immediate query is never verification and never
grounds for rollback on its own; on a first-query failure, sample before
concluding.

BRANCH CONTEXT DOCTRINE (2026-07-30): any repo write, and especially any
cross-estate rollout, verifies the checked-out branch before writing and
confirms the intended branch carries the change after pushing. File content
alone is not verification of a rollout.

# CONTEXT WALL
No Nova asset, colour, key, or copy in this repo, ever, and the reverse. Origin
separation from Nova's tooling is deliberate (resurgam-ltd.github.io vs Nova's
serving origin) so localStorage state never crosses.
Cross-estate forks are stripped BEFORE their first commit to the receiving
repo — pristine copies of another estate's source never enter a receiving
repo's history. Do the fork-strip transformation locally or in a private
scratch area, not inside the destination repo's working tree pre-commit.
(This repo's own history was rewritten 2026-07-24 after Nova identifiers
that predated this rule were found post-commit — see the import commit
message on main for the full account.)

# PUBLIC-REPO DISCIPLINE
This repo is public (GitHub Pages on the free org plan requires it). Secrets,
prospect data, CSV exports, and anything client-identifying never enter it —
.env and data files are gitignored from the first commit.

# resurgam-tool
Single working file: resurgam_prospect_tool_v1.html, forked and stripped from
Nova's prospecting tool. index.html is a thin redirect into it (same pattern
as nova-tool) — the working file is never renamed to index.html itself.
Served via GitHub Pages at https://resurgam-ltd.github.io/resurgam-tool/.
Unannounced during the build phase — nothing links to it and it is not used
on any real prospect until Jamie's click-through sign-off.

Live verification MUST check served content, not status code alone (see
VERIFY BEFORE ASSERT above) — match the build-stamp meta tag / footer
against current HEAD. See KNOWN-GAPS.md for parked issues.
