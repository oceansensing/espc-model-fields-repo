# espc-model-fields-repo — running record

The ESPC **scalar** fields — the cheap half of the model. Created 2026-08-30;
**live since 2026-08-31**. Records from before the products arrived live in
`oceansensing.github.io/PLAN.md` and in `espc-model-repo`'s, which carried the
storage measurements that made this repository necessary.

## Where it stands

Publishes to <https://oceansensing.org/espc-model-fields-repo/> on its own
cron: `17,37,57 * * * *` plus `22 0,3,6,9,12,15,18,21` following the
three-hourly anchor. Three products — `fields-navy`, `ice-navy`, `ssh-navy` —
five roots, four tile tiers.

It holds no code. The orchestrator comes from `realtime-data-repo` and the
fetchers and contract from `oceansensing.github.io`, both checked out at run
time, with `PIPELINE_ROOT` pointing the orchestrator at this workspace.

## 2026-08-31: the migration, and what the first run cost to learn

The five Navy scalar roots moved here from `realtime-data-repo` — `sst-navy`,
`sss-navy`, `sic-navy`, `sit-navy`, `ssh-navy`. The currents had gone to
`espc-model-repo` on 2026-08-22; this is the rest of the model following,
under the split decided 2026-08-30.

**The ice was a live test set up nine days earlier and it passed.** It stayed
behind in August deliberately, to prove that moving one product between
repositories is a declaration rather than a rewrite. What the move cost on the
consumer's side was **one line** in the site's `MAP_ORIGINS`.

**What it cost here was one failed run, and the failure is the useful part.**

The first dispatch fetched everything correctly — all five products, right
grids, right sizes, and it read the currents' published hours cross-origin
from `espc-model-repo` without being told how. Then the write fence refused
the run:

```
FENCE  sic-oisst.json: written by no declared namespace
FENCE  sst-oisst.json: written by no declared namespace
FENCE  ssta-oisst.json: written by no declared namespace
```

`fetch-ocean-fields.py` publishes four families. Three are this repository's;
OISST stayed behind. Invoked bare, the `fields` step fetched all four and
wrote three files into a tree where no product declares them — and a step
writing outside every declared namespace fails the run outright, which is what
that fence is for.

**A product is the unit of OWNERSHIP; a step is the unit of EXECUTION.**
Splitting one script's families across repositories splits the first without
splitting the second. The per-product `namespace`, `tiles` and `tile-key`
commands were already scoped — the step's own `cmd` is the one that is easy to
miss, precisely because in the repository these products came from it had no
reason to be scoped: every family that script wrote had an owner there.

**The mirror image would have been worse and was avoided by one commit.**
Leaving `realtime-data-repo`'s step bare after its products left would have
had it write `sst-navy.json` into a tree that no longer declares it, and its
fence would have stopped **every** product there, in production, on the next
cron. The step's scope and the product list are one change, never two.

**Gated the same day** in the site's `check:docs`, which is the only side that
reads every origin's declaration: a step's namespace set must equal the union
of its products'. Both directions, because they fail differently — a step
scoped too WIDE writes files nobody owns and the fence stops the run loudly; a
step scoped too NARROW never writes files somebody declared, and that one is
silent, carried forward frozen for ever. Asked of the scripts through
`--namespace` rather than a list, so a family added to a fetcher joins the
check. Four mutations killed, including the quiet direction.

## 2026-08-31: storage, measured against the projection

| | projected 08-30 | measured 08-31 |
| --- | --- | --- |
| sst tiles | 44.0 MB | 43.9 MB |
| sss tiles | 45.1 MB | 45.1 MB |
| ice tiles (sic + sit) | 57.6 MB | 54.0 MB |
| **whole tree** | **150.3 MB** | **≈169.5 MB (16.6%)** |

The per-product tile figures were nearly exact. **What the projection
under-counted was the regional grids** — Atlantic, Arctic and Antarctic cuts
at 0.16°, several MB each, which sit in no tile tier and were not in the byte
log the projection was built from. 26.5 MB of them, over 25 files on the
`published` branch.

So the projection was optimistic by about 13%, and it does not matter here:
16.6% against a cap that was the whole reason for the split. It is recorded
because the same method will price the next repository, and there it might.

A cold run with all four tile tiers to build: **3 min 36 s** end to end.

## Open

1. **The upper-ocean heat content layer**, expected to join these five. Its
   upstream fork — ESPC global with a new 3-D read, against ECCOFS regional —
   is the owner's and is not decided. Nothing is started.
2. **`ssh-navy` publishes no forecast frames**, as none of these products do
   today: one hour per run, the one nearest the reader. Whether the scalars
   should bracket the reader the way the currents do has never been asked.
3. **Nothing here gates prose.** There is no `package.json` and no npm; CI is
   a publish run. The mechanism for this repository's documents is the doc
   doctrine's question 6, and the only automated help is the site's
   `check:docs`, which reads this repository's `products.toml` and its
   `CLAUDE.md` doctrine block but not its README.
