# Prism Certification Listing

**Thomas Lincoln Crockett / TLG3D Labs, LLC** — github.com/SerThrocken

This repository is the public listing for the Prism Portability Layer. It holds test suites, signed result files, revocations, and witnessed checkpoints for claims about skill packs and the runtimes that run them.

It exists for one reason: a claim you can check yourself is worth more than a claim you have to take on trust. Every row in this listing traces to a result file in this repository, the suites are frozen and public, and the failures are listed with the passes.

## What this listing certifies — and what it does not

The listing certifies **claims**, never products, people, or safety in the abstract.

A claim is one specific statement: *this pack, at this version and content hash, on this runtime at this version, with this model, passed this suite on this date, under the conditions recorded on its environment card.* Nothing broader is ever stated. There is no "certified runtime" and no "certified pack" — there are only claims, each with its evidence attached.

## How to read a row

Each row in [`listing.md`](listing.md) (machine-readable form: [`listing.json`](listing.json)) is generated from a result file under [`results/`](results/) — the listing is never hand-edited, so a row and its evidence cannot drift apart. A row names:

- **The claim tuple** — pack ID, version, and content hash; or runtime and version; the suite; the model; the tier; the operating system; the network state; and the languages the test cases actually ran in. A claim is only ever for the languages it names.
- **The result** — pass or fail, per case, with the suite version. A suite version bump marks older results historical; they stay visible as history.
- **The age** — every row shows how old its result is, in plain text. A pack that changes has stale claims until it is re-tested. A runtime or model version bump does not downgrade an old row; it creates a new claim tuple holding no claims at all, and the listing says exactly that: *not verified on the current version; last verified on vX, N days ago.*
- **The environment card** — the conditions the result was produced under (below). A result without a complete environment card does not list.

### The environment card

Every result carries one: hardware (CPU, GPU, RAM), operating system and version, runtime and version, model and exact version — for a local model, including the model file's SHA-256 digest, checked against the file on disk at run start — tier, machine load state at run time (idle, or the stated load it ran under), context ceiling, network state (offline / LAN / internet), the hash of the binding set in use, and the languages the cases ran in.

A score without its conditions is advertising. A score with its conditions is evidence you can weigh.

### The two grades

- **Self-tested** — the pack's author ran the published test kit themselves and signed the result file with their author key. The result file's suite hash matches the frozen suite in this repository. Listed with its date and the author's signature.
- **Prism-verified** — the same kit, run on the reference bench (an Acer Nitro: Ryzen 7 7840HS, RTX 4060 8 GB, 64 GB DDR5 — ordinary hardware a normal person can own, on purpose), with the result file threshold-signed **2 of 3** bench keys. No single person or single key can mint a verified result. Listed as verified, with its date.

There is no third grade. Where a self-tested case ran against fixtures that stay on the author's machine (local-only fixtures), the row says **bench-reproducible only** — that limit is printed on the row, not buried.

## How to reproduce a result yourself

You do not need an account, and you do not need to trust this repository's operator. You need the pack, the kit, and the result file:

1. Take the result file for the claim from [`results/`](results/). Note the pack's content hash, the suite name and version, and the environment card.
2. Get the pack and confirm its content hash matches the hash in the result file. A pack whose hash differs is a different pack; its claims are stale by definition.
3. Confirm the suite in [`suites/`](suites/) hashes to the value in `suites/FREEZE-MANIFEST.sha256` for that suite version.
4. Run the kit against the pack: `prism verify` for one runtime, or `prism verify --matrix` for every runtime you have locally. The kit runs offline against the public fixtures.
5. Compare your per-case outcomes with the result file's, case by case. The scorer is deterministic: same inputs, same scorer version, same score.

If your run disagrees with a listed result, that disagreement matters — see [SECURITY.md](SECURITY.md) for how to file it. Verified results are also re-run by third parties on a rotating selection nobody here picks: the selection is fixed by a seeded sort over the newest witnessed checkpoint (see *Witnessed checkpoints* below), so anyone with the checkpoint can reproduce the selection exactly.

The freeze manifest (`suites/FREEZE-MANIFEST.sha256`) also records the scorer's hash — `b3ff2a3c8ceb52e8613deae252267ec03e00cd7237f8404fabf918205c1a7a4a` (for `../../kit/scorer/score.py`). The scorer itself is not exported; the hash is published so you can confirm which scorer version a result was scored with.

## Revocations

A result can be wrong: a scorer bug, a pack found cheating, a claim that no longer holds. Revocation is a new file in [`revocations/`](revocations/) naming the result and the reason. The revoked result **stays visible, marked revoked** — there is no silent delisting, and git history is the audit trail. A pack or runtime caught behaving differently under test than under identical non-test conditions is revoked with that reason named.

Suite definitions themselves are governed just as openly: changing a frozen suite — adding, removing, weakening, or re-scoring a case — requires a public proposal in this repository, a comment window of at least 14 days, and a recorded 2-of-3 threshold sign-off. Changes never apply retroactively; results listed before a change are marked historical. See [CONTRIBUTING.md](CONTRIBUTING.md).

## Witnessed checkpoints

Git history can be rewritten by whoever hosts it, so this listing does not ask you to trust the host. The listing publishes a **signed checkpoint** of its own state at least weekly, and at every verified result. Independent witnesses — other runtimes, kit operators, any third party — co-sign the checkpoints they have seen and keep their own copies, under [`checkpoints/`](checkpoints/). A rewritten history fails against the witness copies, in the open. The record is trustworthy because rewriting it would be evident, not because the host is trusted.

## If something goes wrong

Any confirmed security incident, wrong listed result, or scorer defect gets a public entry in this listing **within 7 days of confirmation**, stating the fix — or stating plainly that no fix exists yet. That deadline is a rule this project set for itself in advance, not a promise made after the fact.

Found a break, or built a pack designed to break a listed claim? There is a standing invitation: a successful break is published here with your name on it, and the broken claim is revoked with your attack cited. See [SECURITY.md](SECURITY.md).

## What we claim, in exact words

This system is **compromise-evident, never compromise-proof** — and that wording is a rule, not a slogan. No page, listing, document, or badge from this project may claim a system is unbreakable, unhackable, or compromise-proof — those words appear in this README only inside this sentence, which states what we *refuse* to claim. What the design does claim is narrower and checkable: breaking a listed claim has to defeat several independent mechanisms, and having done so, the break is visible in this public record and the claim is revocable.

The known limits are published with the design, not saved for later discovery: prompt injection is mitigated by the refusal suite, least-privilege execution, and human gating — it is **not solved**, here or anywhere, and any wording anywhere in this project that implies otherwise is a defect to file. One phished operator is the cheapest attack on any of this; threshold signing caps what one compromised person or key can do. Whoever owns the bench hardware owns the bench; split custody and public reproducibility limit the value of that ownership rather than removing it.

## Who this is for

- **Homelab and self-hosting households first.** Your automations should not die because a vendor's cloud did. Packs listed here run locally, export completely, and carry receipts you can check — and this listing is how you check what a pack actually did, on hardware like yours, before it runs in your house.
- **Framework and runtime developers second.** You rebuild the same tools per framework and get audit trails nobody can verify. Here, one pack runs unchanged across runtimes, the differences show up as published rows — disagreements between honest implementations are published as findings — and an adapter is four interfaces: load a pack, present its tools, execute it, emit a run record.

## Licence

The contents of this repository are covered by the licence in [`LICENSE`](LICENSE) at the repository root. Author attribution and licence terms survive every conversion in this system, machine-readably; stripping them fails the kit's checks and voids certification.
