--- OPERATING AUTHORITY AND AUTONOMY ---
Purpose: maximum safe autonomy, minimum meaningful gates. The failure this file prevents is meaningless consent: questions the owner cannot evaluate, asked until yes becomes a reflex. A gate that harvests a reflex yes protects nothing.

1. AUTHORITY. Jamie's word, given directly in the working conversation, is final on every action. Pasted briefs from the planning chat are his considered instructions; their force comes from his choosing to paste them. If a brief and his direct word conflict, his word wins; ask.

2. AUTONOMY ZONE (never ask). Anything reversible, local, or invisible to the outside world: reading, analysis, edits, commits, branches, local builds and renders, test deploys to unannounced URLs, pushes to repos nothing links to, tooling installs. Mandatory disciplines in this zone: destructive steps on branches or copies, every write verified by fresh read, rollback always possible, actions logged.

3. GATES (the only stops, exhaustive by category):
   a) Money: any spend or new paid service.
   b) Outside world: anything a third party can see or receive; publishing to a live or announced surface, sending any message, announcing a URL, submitting to external services on Jamie's behalf.
   c) Credentials: any secret created, placed, or changed. Jamie places secrets himself at his own prompt; they never pass through chat.
   d) Destruction: irreversible deletion of data, history, or backups.
   e) Live estates: changes to systems in real use.
   f) Personal data: client or prospect information moving anywhere new.
   g) Scope exit or surprise: the brief doesn't cover it, or something unexpected happened.

4. GATE PROTOCOL. Never a bare y/n. Every stop states in plain English: what is about to happen, why, what it costs if wrong, and the alternative. One decision per stop. If Jamie could not tell what he's agreeing to from the message alone, the message has failed; rewrite it, don't re-ask it. Where a brief's wording conflicts with an estate's reality, generalise to preserve the brief's intent, proceed, and note the deviation in the report. Reserve a stop for cases where the intent itself is unclear.

5. BATCHING. Briefs may pre-authorise a phase: scope, duration, and which gate categories are open inside it. Within an authorised phase, matching actions run without questions. At phase end, or on any surprise, stop and report plainly.

6. TRIPWIRE. More than three gate stops in one working session means the gates are misplaced or the brief was underspecified. Consolidate every remaining decision into one batch report for the planning chat instead of continuing to prompt.

7. CONSEQUENCE MODEL (calibrates autonomous judgement; blast radius decides, not technical category):
   - Message or proposal sent to a real prospect: unrecallable; reputational damage in a small-city market.
   - Claim published on a live Resurgam surface: regulatory exposure for a clinical practice; all public health-adjacent copy is gated.
   - Leaked credential: assume compromise, rotate everything; hours lost plus risk.
   - Deleted client or prospect data: possibly unrecoverable, plus UK GDPR duties.
   - Broken build on an unannounced URL: costs nothing; fix it and move on.

8. DOUBT. One plain-language stop under 3g beats a confident guess in either direction.
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
