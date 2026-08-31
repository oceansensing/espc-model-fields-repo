# espc-model-fields-repo — running record

The ESPC **scalar** fields — the cheap half of the model. Created 2026-08-30;
**live since 2026-08-31**. Records from before the products arrived live in
`oceansensing.github.io/PLAN.md` and in `espc-model-repo`'s, which carried the
storage measurements that made this repository necessary.

## Where it stands

Publishes to <https://oceansensing.org/espc-model-fields-repo/> on its own
cron: `17,37,57 * * * *` plus `22 0,3,6,9,12,15,18,21` following the
three-hourly anchor. **Five products, seven roots, five tile tiers** —
`fields-navy`, `ice-navy`, `ssh-navy`, `temp30-navy` and `ohc-navy`.

It holds no code. The orchestrator comes from `realtime-data-repo` and the
fetchers and contract from `oceansensing.github.io`, both checked out at run
time, with `PIPELINE_ROOT` pointing the orchestrator at this workspace.

## 2026-08-31: ocean heat content, and a 30 m temperature that came free

Two products, seven roots. Both off `water_temp` on `ts3z`, both asked for by
the owner in one breath: a heat content layer for hurricane intensity, and —
since a 3-D read was needed either way — a temperature at 30 m alongside it.

**The quantity is tropical cyclone heat potential**, integrated to the 26 °C
isotherm rather than to a fixed depth. The design, the measurement that chose
a 300 m read, and the three answers the reduction gives are in the site's
`PLAN.md` and in `DECISIONS.md` D2; what follows is what this repository
measured.

### The first run, and the case that fired on it

| | |
| --- | --- |
| `ohc-navy.json` | 326 KB, **11,819** cells with a D26, max **224.3 kJ/cm²** |
| `ohc-navy-atlantic.json` | 851 KB, 83,473 cells |
| `temp30-navy.json` | 317 KB, 45,719 cells |
| `temp30-navy` tiles | 158 tiles (4 all land), **43.5 MB** |
| run | **3 min 16 s** including the new tier cold |

**224.3 kJ/cm² is the number to sanity-check, and it checks.** At 0.41 kJ/cm²
per K·m that is 546 K·m — a mean excess of about 2.9 K over roughly 190 m,
which is a deep warm pool and exactly the kind of column the read depth
argument was about. The deepest-D26 columns are the highest-heat ones, which
is why truncating them would have mattered more than their count suggests.

**One column exceeded the read, on the very first run:**

```
! ohc-navy: 1 column(s) never reached 26 C above 300 m, so the read did not
  contain D26 — published absent rather than understated
```

That is 1 in about 11,820 warm cells, 0.008%. The coarse sample taken before
the build found none in 2,063 columns and its deepest D26 was 191 m — so the
sample was optimistic in exactly the way it was predicted to be, and the
choice of 300 m over the 200 m the sample alone would have justified is what
kept the count at one.

**It is not a reason to deepen the read again**, and saying why matters more
than the number: there is always a deeper column somewhere. What this run
demonstrated is that the third answer works — the column is a **reported
hole** rather than a silent underestimate, which is the whole design. The
depth only tunes how often it fires. If the count climbs into the tens, 400 m
is two levels away.

### 2026-08-31, later: 0.08° where the storms are

The owner's call after the cost was priced. Atlantic & Gulf at the model's own
0.08°, Pacific and Indian at 0.16°, the 0.96° global kept as the overview.
Measured on the first run, against what was predicted before building it:

| grid | predicted | published |
| --- | --- | --- |
| Atlantic 0.08° | 3.47 MB | **3.39 MB** (1126×626) |
| Pacific 0.16° | 2.94 MB | **2.92 MB** (1188×501) |
| Indian 0.16° | 1.04 MB | **1.02 MB** (563×376) |

**The refused option is the instructive one.** A literal 0.16° *global* grid
would be 11.9 MB in one file the reader downloads the moment the layer is
switched on, against 326 KB today, and 481 MB a build of which three quarters
is water with no 26 °C isotherm. **A grid is an all-or-nothing download** —
which is exactly what makes a coarse global one the right overview and a fine
one the wrong everything, and it is why this went to regions rather than to a
finer global stride.

**`tileLat` (±45°) is declared and inert**, reserving the option to go finer
still. `build_tile` cannot know a box is empty without fetching it, so an
unbounded 0.08° tier here would read all 162 boxes at 25 levels to discover
105 hold no isotherm: 1.94 GB a build to publish 0.7 GB worth. Bounded, 108
boxes. Turning tiles on is one word now rather than a bandwidth surprise.

**A finer grid finds more truncated columns, which is the expected direction.**
The run reported 1 on the global grid and 9 on the Atlantic at 0.08° — of
339,415 wet cells there — because a coarse cell averages away the deepest warm
cores that a fine one resolves. Still absent-and-reported rather than
understated.

### 2026-08-31, later still: the tier that has no gaps

The 0.08° Atlantic region was reported coarse over the Gulf twice — at zoom
5.1, then again at 4.8 further west. Both times the data was right and the map
would not ask for it: **a region is used only when the whole viewport fits
inside it**, and a Gulf-centered view pokes out of the Atlantic box's western
edge and the Pacific box's eastern edge at the same time.

**Widening boxes does not converge**, which is what made this a design
question rather than another number. At the region minZoom of 4 a viewport is
127° wide, so containing every view needs a box ~63° larger than its basin on
each side — for the Atlantic, most of the globe.

**A single 0.08° band over all longitudes was priced and refused.** It fixes
longitude for free, since spanning 360° takes `regionCovers`'s `spansWorld`
branch — but it moves the same non-convergence to latitude, where a zoom-4
view is 63° tall and escapes any sane band directly over the Gulf Stream. It
also costs 1.0 GB a build against tiles' 1.35, and hands the reader **25 MB in
one blocking fetch** against a tile viewport's 5.5 MB.

| | published | HYCOM/build | worst single fetch | gaps |
| --- | --- | --- | --- | --- |
| regions | 7.8 MB | 0.32 GB | 4.9 MB | **yes** |
| one 0.08° band ±45 | 24.8 MB | 1.01 GB | **24.8 MB** | in latitude |
| **0.08° tiles ±45** | 33.1 MB | 1.35 GB | **5.5 MB** | **none** |

So the tier costs ~4× the regions upstream and is the only one that answers
the requirement. `tileLat`, reserved the previous evening against exactly this
possibility, is what keeps it at 108 boxes instead of 162.

The Pacific and Indian regions went with the change: added that morning to
lift those basins off the globe, superseded the same day by a tier that does
it everywhere, and 160 MB a build to keep as a fallback nothing would reach.

### Storage

The tree is **≈219.7 MB, 21.5%** of the cap: 186.5 MB of tiles and 33.2 MB of
grids and status. It was 169.5 MB before these two, so they cost about 50 MB —
almost all of it `temp30`'s tile tier. `ohc` takes none, on sea surface
height's argument.

### What the pair cost each other, which is the reusable part

**Almost nothing, and that was the point of doing them together.** The heat
content reads levels 0–24 whatever happens; 30 m is index 10, inside that
span. Publishing it is one more grid and one more tier, no extra request —
the same trade the currents made when they moved their second layer from 60 m
to 50 m to land inside a subset the depth-averages already needed.

They are still **two products**, because fates follow upstream reads and each
issues its own. `ohc-navy` is the costliest read here — 25 levels against one
— so it is the most likely of the five to fail alone, which is exactly the
containment a product is for.

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

1. ~~The upper-ocean heat content layer.~~ **Built 2026-08-31**, on ESPC:
   ECCOFS stops short of the main development region, so a storm's heat
   potential along most of its track would be missing. See above.
2. **`ssh-navy` publishes no forecast frames**, as none of these products do
   today: one hour per run, the one nearest the reader. Whether the scalars
   should bracket the reader the way the currents do has never been asked.
3. **Nothing here gates prose.** There is no `package.json` and no npm; CI is
   a publish run. The mechanism for this repository's documents is the doc
   doctrine's question 6, and the only automated help is the site's
   `check:docs`, which reads this repository's `products.toml` and its
   `CLAUDE.md` doctrine block but not its README.
