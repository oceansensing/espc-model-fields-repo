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
nothing was missing but the file, which is how the site went seven weeks
without one and `realtime-data-repo` eighteen days. **This block asserted
otherwise from the day it was written** — byte-compared in eight places and
false in two of them, because a gate on a text is a gate on the text. What it
cost is measurable: the engine promotion's own rehearsal listed *"a dated
entry in this repo's decisions and oceanlet's"* as its ninth step, and the
half with nowhere to go was simply not written.

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
4. Did a one-way door close — **or has one already recorded stopped being
   fully true**? → `DECISIONS.md`, in **every** repository the change
   touched. All eight carry one since 2026-08-31, so this is no longer the
   engine's question with seven exemptions; the amendment half is here
   because two entries needed one within a day of being written.
5. Did an interface, a deliberate divergence, or a concept the guide explains
   move? → the matching file under `docs/`
6. **Does a document in another repository now say something false because of
   this change?** → fix it there, in the same sitting.

**Question 6 is the one that gets missed, and it is why this block is
identical in eight places.** Measured 2026-08-28: one tile-tier measurement
falsified `espc-model-repo`'s README, its `products.toml` header and the
site's README at once. Two were found; the third took a reminder from the
owner, who then asked for this doctrine.

**Two repositories are deliberately NOT in the list above, on opposite
grounds, and both are named because an exclusion nobody wrote down is
indistinguishable from an oversight.**

`ocean-now`, the iOS port, **consumes this system** — it mirrors the site's
published contract. It is not swept by these six questions and does not carry
this block; it has a lighter mechanism instead, a pending list in its parity
ledger, and the two repositories whose changes can reach it (the engine and
the site) each say so in their own section. It is named here because "four"
was read as "all of them" for two weeks while that ledger drifted 176 commits
behind with nothing noticing — question 6 failing at the granularity of a
whole repository rather than a document.

`hab-data-repo` is excluded on the opposite ground: **it does not touch the
ocean map at all** (the owner's call, 2026-08-31). It publishes the bloom
photographs for a different part of the website, reached through `HAB_DATA`
in `src/config.ts`, and carries no interface anything here codes against
beyond a URL and a filename convention. It needs no mechanism, not even a
lighter one — nothing in these eight can falsify a claim in it, and it cannot
falsify one here. Do not mix it in.

Adding a repository to the list above is therefore a real act: it buys the
sweep, and leaving one off **silently** costs exactly what `ocean-now` cost.

A number in prose is only as good as its anchor. `check:docs` gates every
claim it can tie to a source constant and nothing else, so when a figure has
no anchor — a measurement, a live reading, a byte count off a build log —
write **where it was measured and when**, or the next reader cannot tell a
fact from a guess that aged.
<!-- DOC-DOCTRINE v1 end -->

### In this repository

- **`README.md`** — what this publishes, the measured storage, how to run it.
- **`PLAN.md`** — the running record: what happened here, measured, and what
  is open.
- **`DECISIONS.md`** — dated one-way decisions, D1 onward.
- **`pipeline/products.toml`** — the declaration, and the only thing here that
  the pipeline reads.

## What must not be got wrong here

### The step's scope and the products' scope are ONE change

**`fetch-ocean-fields.py` publishes four families and this repository owns
three.** The `fields` step therefore carries
`--only=sst-navy,sss-navy,sic-navy,sit-navy,ssh-navy`, and it has to.

Invoked bare it fetches OISST too, writes three files no product here
declares, and the write fence refuses the run — which is what happened on the
first dispatch, 2026-08-31. **A product is the unit of OWNERSHIP; a step is
the unit of EXECUTION.** Adding or removing a product here means asking
whether the step's `--only` still matches, in the same commit.

The site's `check:docs` holds this now, in both directions and across every
origin. Too WIDE is loud (the fence stops the run); too NARROW is silent — the
files are simply never written and the previous copies carry forward frozen
for ever, which is the shape three other entries in this project are about.

### The ESPC hour rule is cross-origin and permanent

One model run publishes one hour, and its members span **three** repositories
now: the currents in `espc-model-repo`, these five here, and nothing in
`realtime-data-repo` since 2026-08-31. No repository can enforce that alone.
Only the site can, reading every origin's manifest.

It works because the anchor is a pure function of time — the origins agree
*without coordinating*, and what splitting cost was the gate, not the
agreement. The arrangement that would let one pipeline check it is all ten
roots in one repository, which is exactly what storage forbids. Permanent, not
transitional.

### Two rules inherited from the sibling data repositories

- **Every `roots` entry in `products.toml` must be one the site's
  `test-schema.mjs --roots` publishes**, or the orchestrator exits 2 and stops
  the publish. It does *not* fail on the reverse: a contract root this
  repository has no product for belongs to another origin.
- **A product that leaves takes its files with it.** The stage is seeded from
  the last publish, so a root a pipeline stops writing is carried forward for
  ever and served frozen. That is `realtime-data-repo`'s side of this
  migration and it cost 21 files there.

### Reading a run

`status/status.json` is the health record and the routing document both: each
product carries its fate, its reason if held, and the `roots` this origin
serves, which is what the map routes on.

**`checked` and `updated` are different questions.** `checked` is the last
time the pipeline successfully attempted the product; `updated` is the last
time its bytes changed. One product's frozen `checked` beside a current
`generated` is the ordinary signature of a hold, not of a stopped pipeline.
`generated` at the top of the document is the run that actually ran, and it is
the field to read first.

**When the tree is frozen, the run log is the only current account of why** —
`status.json` is published *with* the tree, so a `deploy=False` leaves readers
fetching the last run that succeeded.

### The checkout that everything depends on

This repository reads the **private** site repository for its fetchers, and a
read-only deploy key is the only reason it can — `PIPELINES_SSH_KEY` here,
public half `espc-model-fields-repo-checkout` on the site repository. Its own
key, not the one another repository uses, so revoking or rotating one does not
take the other down.

**The hazard is the ordering, and it has been measured once already**: the
secret *arms* the SSH path the moment it exists, because a non-empty `ssh-key`
sends `actions/checkout` down SSH and the HTTPS fallback is never consulted.
Key on the site repository first, secret here second.

## The working agreement

The same one the sibling repositories keep: a measured constant moves with its
reason in the same commit; new checks are mutation-tested before they are
believed; exit codes are captured before output is read; docs are part of the
change, never a follow-up. **A number in prose without an anchor says where it
was measured and when, or it is a guess that will age.**
