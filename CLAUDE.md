# espc-model-fields-repo

The ESPC **scalar** fields — the cheap half of the model. Nothing is built; `README.md` says what the source is and
`PLAN.md` is the founding plan.

<!-- DOC-DOCTRINE v1 begin — identical in all eight repositories; `check:docs` holds them equal. Edit one, sync all. -->
## Where truth lives, and what "update docs" means

Eight repositories carry this project. The engine and the site:
`oceanlet.js`, `oceansensing.github.io` (the site, and every fetch script).
The orchestrator and the observations: `realtime-data-repo`. And the data
repositories, which since 2026-08-30 split **currents from fields** per model:
`espc-model-repo` (the ESPC currents — a legacy name, see below),
`espc-model-fields-repo`, `eccofs-model-currents-repo`,
`eccofs-model-fields-repo`, and `sentinel3-data-repo` (ocean color, which has
no vector half to split). Each document answers exactly one question.

**`espc-model-repo` is the ESPC CURRENTS repository** despite its name — the
one exception to the convention, kept because its URL is a live origin and
GitHub Pages does not reliably redirect a renamed project site. Read it and
`eccofs-model-currents-repo` as the same kind of thing.

*(`eccofs-model-repo` was RENAMED to `eccofs-model-fields-repo` on 2026-08-30,
not superseded — GitHub redirects the old name, which is why a rename was
free there and is not free for `espc-model-repo`: that one has published
bytes behind a Pages URL, and Pages does not redirect what the API does.)*

**All eight carry the same four documents, and since 2026-08-31 a gate holds
them to it** — `check:docs` requires a `DECISIONS.md` tracked in git in every
repository. The last two landed that day, the site's and
`realtime-data-repo`'s, reconstructed from records that already existed:
nothing was missing but the file, which is exactly how the gap lasted seven
weeks. What it cost is measurable — the engine promotion's own rehearsal
listed *"a dated entry in this repo's decisions and oceanlet's"* as its ninth
step, and the half with nowhere to go was simply not written.

| file | answers | tense | it is stale when |
| --- | --- | --- | --- |
| `README.md` | what this is, how to run it | present | a reader types a command or trusts a number and is wrong |
| `CLAUDE.md` | what must not be got wrong here | imperative | the next session is about to repeat a mistake |
| `PLAN.md` | what happened, measured, and what is open | dated past | "why is it like this?" has no answer here |
| `DECISIONS.md` | which one-way door closed, and when | dated | a reversal would cost a migration and nothing says so |
| `docs/` | contracts, ledgers and the guide | present | it describes an interface, a divergence or a concept that has moved on |

**`docs/` is a first-class part of "all docs", not an appendix** — the owner
asked for that explicitly on 2026-08-28, and the reason is that these are the
documents everything else points AT. A frozen contract, a divergence ledger
whose rows are pinned by tests, a guide that introduces the model: each is
the thing a reader is sent to when the short answer will not do, so each is
the worst place for a claim that has quietly stopped being true.

**"Update docs" means a sweep of all eight repositories, not the one in hand.**
Docs are part of the change, never a follow-up and never a separate ask. Six
questions, asked of every repository the change touched:

1. Did a command, a path, a script name or a number a reader would type or
   trust move? → `README.md`
2. Did a rule, a trap, or a things-that-must-move-together change or come to
   light? → `CLAUDE.md`
3. Did something *happen* — a measurement, a defect, a yield, a mechanism, an
   open question opened or answered? → `PLAN.md`
4. Did a one-way door close? → `DECISIONS.md`
5. Did an interface, a deliberate divergence, or a concept the guide explains
   move? → the matching file under `docs/`
6. **Does a document in another repository now say something false because of
   this change?** → fix it there, in the same sitting.

**Question 6 is the one that gets missed, and it is why this block is
identical in eight places.** Measured 2026-08-28: one tile-tier measurement
falsified `espc-model-repo`'s README, its `products.toml` header and the
site's README at once. Two were found; the third took a reminder from the
owner, who then asked for this doctrine.

**One more repository consumes this system and is deliberately NOT in the
list above**: `ocean-now`, the iOS port, which mirrors the site's published
contract. It is not swept by these six questions and does not carry this
block — it has a lighter mechanism instead, a pending list in its parity
ledger, and the two repositories whose changes can reach it (the engine and
the site) each say so in their own section. It is named here because "four"
was read as "all of them" for two weeks while that ledger drifted 176 commits
behind with nothing noticing — which is question 6 failing at the granularity
of a whole repository rather than a document. Adding a repository to the list
above is therefore a real act: it buys the sweep, and leaving one off costs
exactly what that cost.

A number in prose is only as good as its anchor. `check:docs` gates every
claim it can tie to a source constant and nothing else, so when a figure has
no anchor — a measurement, a live reading, a byte count off a build log —
write **where it was measured and when**, or the next reader cannot tell a
fact from a guess that aged.
<!-- DOC-DOCTRINE v1 end -->

### In this repository

- **`PLAN.md`** — the founding plan and running record.
- **`DECISIONS.md`** — dated one-way decisions, D1 onward.
- **`pipeline/products.toml`** — not written yet.

## What must not be got wrong here

### These products are still somewhere else

**The five roots this repository is for are published by `realtime-data-repo`
today and have not moved.** Until they do, they are that repository's
responsibility — do not treat them as this one's because a plan says they are
coming. That is how a product ends up owned by nobody.

**When the move happens: a product that leaves takes its files with it.** The
stage is seeded from what is already published, so a withdrawn product lingers
in the old origin unless it is deliberately removed. That has bitten before.

### Two rules inherited from the sibling data repositories

- **Every `roots` entry in `products.toml` must be one the site's
  `test-schema.mjs --roots` publishes**, or the orchestrator exits 2 and stops
  the publish.
- **A product that leaves takes its files with it.**

### The ESPC hour rule is cross-origin and permanent

The contract requires one model run to publish one hour across every ESPC
member, and those members span more than one repository — so no repository can
enforce it alone. Only the site, reading all origins, can. The arrangement
that would fix it (one repository for all of them) is exactly what storage
forbids.

## The working agreement

The same one the sibling repositories keep: a measured constant moves with its
reason in the same commit; new checks are mutation-tested before they are
believed; exit codes are captured before output is read; docs are part of the
change, never a follow-up. **A number in prose without an anchor says where it
was measured and when, or it is a guess that will age.**
