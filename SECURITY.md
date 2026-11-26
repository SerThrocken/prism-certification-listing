# Security — Reporting a Break, Submitting an Attack Pack

**Thomas Lincoln Crockett / TLG3D Labs, LLC** — github.com/SerThrocken

## 1. Report a break privately first

Found a way to defeat a listed claim, forge or alter a result file, defeat the negotiation check, break the run-record hash chain, game a suite or its scorer, or bypass a witnessed checkpoint? Report it before you publish it:

- Open a **private security advisory** on this repository (GitHub: *Security → Report a vulnerability*), or — for a conduct matter — the same channel marked "conduct" (see CODE_OF_CONDUCT.md).
- Include: the claim tuple or component affected, the result file(s) involved by path and hash, the steps to reproduce, and the run records in **shared form only** (hashes and templated summaries — never payloads, never credentials; spec §5.3).
- You will get an acknowledgement. A confirmed issue becomes a public entry in the listing record **within 7 days of confirmation**, stating the fix or stating plainly that no fix exists yet (§11.12) — that deadline binds this project, and it starts at confirmation, so a report that sits unacknowledged is itself a failure you may cite.

Do not file breaks as public issues before the private report, unless the issue is already public knowledge or actively causing harm that a public warning would reduce.

## 2. Submit an attack pack (the standing invitation, §11.9)

Alongside private reporting, this project runs a standing public invitation: **anyone may submit a pack designed to break a certified claim.**

- Build the pack to the published pack format, targeting a named listed claim.
- Submit it by pull request under `attacks/<claim_id>/`, with: the target claim tuple, what the pack does in plain words, and the result you expect if it succeeds.
- A successful break is **published in the listing with the submitter's name on it**, and the broken claim is **revoked with the attack cited** (§4.6, §11.9). Credit is the deal, and it is kept.
- Test-detection is a break by definition: a pack or runtime that behaves differently under the verifier's stated conditions than under identical non-test conditions fails the refusal suite and is revoked with that reason named (§4.8).

## 3. Safe harbour

Good-faith security research under this policy is welcome and will not be pursued by TLG3D Labs, LLC, provided the research:

- targets only the published kit, suites, listing data, and packs the researcher owns or has permission to test — never another user's live system, household, or data;
- runs untrusted or hostile packs only inside the kit's sandbox, with no production credentials mounted (§11.4);
- does not access, alter, or destroy data that is not the researcher's own, and stops and reports if it reaches any;
- follows §1 (private first report) for anything that defeats a listed claim, before public disclosure;
- complies with applicable law. This draft states the project's intent and practice; it does not change anyone's legal rights or obligations.

Research that stays inside these lines and breaks something is a contribution, and it will be credited as one.

## 4. What this project's security claim actually is

Compromise-evident, never compromise-proof (§11). The mechanisms are public so they can be checked, not admired: threshold signing 2 of 3 (§11.3), brokered capabilities with no ambient authority for packs (§11.4), canary packs whose failures are listing events (§11.5), witnessed checkpoints (§11.8), public re-runs on a selection the bench cannot pick (§11.9), and a trusted core kept small enough to audit (§11.10). Residual risks — the model, the human, the dependency pile, physical access to the bench — are named in the spec (§11.6) and summarised in the README. If you find a place where this project's wording claims more than this section does, that wording is a defect: file it like any other.
