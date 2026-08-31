# espc-model-fields-repo

The ESPC **scalar** fields — the cheap half of one model — published for the
[C4PO ocean map](https://oceansensing.org/visualization/) at
<https://oceansensing.org/espc-model-fields-repo/>. A sibling data repository:
its own Pages site, its own cron, its own gigabyte, holding no code of its own.

**Live since 2026-08-31**, when the five roots moved here from
`realtime-data-repo`. The currents come off the same US Navy ESPC-D-V02 run
and are published by `espc-model-repo` — which is the ESPC **currents**
repository despite its name, a legacy exception kept because its URL is a live
origin that a rename would 404.

`CLAUDE.md` is the operator's and maintainer's half — what to run, what must
move together, what has already gone wrong. `PLAN.md` is the running record.
`DECISIONS.md` indexes the dated one-way decisions. Which document gets what,
across all eight repositories of this project, is the doctrine block at the
top of `CLAUDE.md`.

## What it publishes

Five products, seven roots, one HYCOM read per product — which is why they
are five products and not one: **fates follow upstream reads.**

| product | roots | tiles | cadence |
| --- | --- | --- | --- |
| `fields-navy` | `sst-navy.json`, `sss-navy.json` | `tiles-sst-navy`, `tiles-sss-navy` | 3-hourly |
| `ice-navy` | `sic-navy.json`, `sit-navy.json` | `tiles-sic-navy`, `tiles-sit-navy` | 3-hourly |
| `ssh-navy` | `ssh-navy.json` | none, by design | 3-hourly |
| `temp30-navy` | `temp30-navy.json` | `tiles-temp30-navy` | 3-hourly |
| `ohc-navy` | `ohc-navy.json` | `tiles-ohc-navy` | **6-hourly** |

**Every product takes the hour the currents publish** — that is what keeps one
model on one hour across three repositories — **except `ohc-navy`, which
floors it to a 6-hourly step.** So heat content updates four times a day, at
00/06/12/18Z, and is either level with its siblings or exactly three hours
behind them. Never anything else: `DECISIONS.md` D4 has why, and the site's
contract checks the relation rather than exempting the layer from it.

**`ohc-navy` is ocean heat content — tropical cyclone heat potential**, in
kJ/cm²: ρ·c_p times the integral of (T − 26 °C) from the 26 °C isotherm to the
surface, off a 0–300 m profile read. **An absent cell means there is no 26 °C
isotherm there, not that the heat is zero** — the question does not apply, and
zero would drag the color ramp across every cold ocean. `DECISIONS.md` D2 has
the rest of the published shape.

**Its resolution is three tiers** (2026-08-31):

| tier | resolution | what a reader fetches |
| --- | --- | --- |
| global overview | 0.96° | 326 KB |
| Atlantic & Gulf | 0.08° | 4.9 MB, only inside the box |
| **tiles** | **0.08° everywhere D26 exists** | ~5.5 MB per viewport |

**The tiles are the tier that matters and the region is their fallback.** A
region is used only when the *whole viewport* fits inside it, so every zoom
and center that straddles two boxes falls back to the 0.96° globe — reported
twice off the live map over the Gulf. Widening boxes does not converge: at the
region minZoom of 4 a viewport is 127° wide.

A tile mosaic is assembled per viewport and needs no containment in either
axis, so it has **no resolution gap at any zoom or center**. It costs about
**1.35 GB a build** off HYCOM, because a box is 25 levels rather than one and
`build_tile` must fetch a box to learn whether it holds anything; `tileLat`
bounds it to the 108 boxes that can hold a 26 °C isotherm.

**`temp30-navy` is temperature at 30 m**, index 10 of the profile the heat
content reads anyway — so it costs one grid and no extra request. Below the
diurnal skin and most of the wind-mixed layer, still inside the water a storm
overturns.

Each root also publishes its regional cuts at 0.16° — Atlantic, Arctic,
Antarctic — resolved relative to the file that names them, so a product
carries its location with it.

**`ssh-navy` has no tile tier and that absence is deliberate**: a free surface
is a smooth field, and the region stride already resolves the mesoscale eddies
a reader looks for.

## Storage, measured on the first run

| | |
| --- | --- |
| tile tiers | **186.5 MB** — sst 43.9, sss 45.1, sic 38.7, temp30 43.5, sit 15.3 |
| grids and status (the `published` branch) | **33.2 MB** over 33 files |
| **total tree** | **≈219.7 MB, 21.5%** of the 1 GB Pages cap |

Measured 2026-08-31 after the heat content and the 30 m temperature landed;
the five-product tree before them was ≈169.5 MB.

From the runs' own logs and the branch's object sizes. **The 2026-08-30
projection was 150.3 MB for the original five roots and it was optimistic by
about 13%** — the per-product tile figures it was built from were close
(sst 44.0 projected against 43.9 measured, sss 45.1 against 45.1) and what it
under-counted was the regional grids, which are several MB each and are not in
a tile tier at all.

There is room for the planned upper-ocean heat content layer and a great deal
besides. **The reason this repository exists is that there was not**: the five
roots plus the currents in one repository is 982 MB, 96% of the cap, less than
one current frame of margin.

## How it runs

**No code here.** The orchestrator (`pipeline/orchestrate.py`) is checked out
from `realtime-data-repo`; the fetchers and the published-file contract
(`schema.ts`) from `oceansensing.github.io`. `PIPELINE_ROOT` points the
orchestrator at this workspace, so it assembles and publishes this
repository's tree from this repository's declaration. A change to a fetcher or
to the contract lands here on the **next run**, not on any push here.

This repository carries `pipeline/products.toml` — what it publishes — and
nothing else executable.

- **Dispatch a run**: Actions → the publish workflow → Run workflow. One kind
  of run; it fetches whatever the anchor has moved past.
- **Read the health**: `curl -s https://oceansensing.org/espc-model-fields-repo/status/status.json | python3 -m json.tool`
- **Scheduled**: `17,37,57 * * * *`, offset from `realtime-data-repo`'s
  `3,23,43` and `espc-model-repo`'s `7,27,47` so no two of the three read
  HYCOM in the same minute, plus `22 0,3,6,9,12,15,18,21` — a dedicated
  attempt just after each three-hourly anchor rollover, which is the only
  moment there is genuinely new work.

A cold run with every tile tier to build measured **3 min 36 s** end to end,
and the run that added the heat content and its 30 m by-product — a 25-level
profile read plus a new tile tier — took **3 min 16 s** (2026-08-31). A run with nothing new to fetch exits after a handful of OPeNDAP
metadata requests.

## What publishes where

```
oceansensing.org/espc-model-fields-repo/
  map/          the grids, their regional cuts and the four tile tiers
  status/
    status.json one object per product: fate, reason, checked, updated, hour,
                and the roots this origin serves — which is what the map
                routes on
    plan.json   what this run intended before it started
    receipt.json  what it actually did
```

The `map/` layout is the contract defined by `schema.ts` in the site
repository, and the site's own `test-schema.mjs` runs over the assembled tree
before anything deploys. The contract is the consumer's, deliberately.

## Structure

```
PLAN.md               the running record: what happened, measured, what is open
CLAUDE.md             what must not be got wrong, and the shared doc doctrine
DECISIONS.md          dated one-way decisions, D1 onward
index.html            the one hand-written page; Pages serves it at the root
pipeline/
  products.toml       what this repository publishes, and nothing executable
.github/workflows/
  publish.yml         thin YAML around an orchestrator that lives elsewhere
```

## License

The code here is the owner's; the scientific data belongs to the body that
produced it. The fields are **US Navy ESPC-D-V02**, served through HYCOM, and
credited on the map itself. The repository is public so the data can be served
from GitHub Pages; that is not a grant of any license.
