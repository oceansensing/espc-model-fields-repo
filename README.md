# espc-model-fields-repo

The ESPC **scalar** fields — the cheap half of the model. A sibling data repository: its own Pages site, its own cron,
its own gigabyte, holding no code of its own.

**Nothing is built.** `PLAN.md` is the founding plan; `CLAUDE.md` carries what
must not be got wrong and the shared doc doctrine.

## What it will publish

SST, SSS, SSH, sea-ice concentration and thickness, and eventually an
upper-ocean heat content layer. **Five roots today**: `sst-navy.json`,
`sss-navy.json`, `ssh-navy.json`, `sic-navy.json`, `sit-navy.json`.

**They are still in `realtime-data-repo` and have not moved.** This
repository exists so the move has a destination; the migration is a separate
sitting and nothing here is wired yet.

## Storage

150.3 MB for the five (**14.7%** of the 1 GB Pages cap), or 195.3 MB
(19.1%) once a heat-content layer joins them — computed 2026-08-30 from
`espc-model-repo`'s own byte log. They could not move into that repository
because currents plus scalars is 982 MB, **96%**, less than one current frame
of margin. That is the whole reason this repository exists.

## Why it is separate from `espc-model-repo` — which is the ESPC **currents** repository despite its name

**Every model splits two ways along the axis that costs bytes** (decided
2026-08-30): a currents repository for the tiled vector fields, which are
expensive, and a fields repository for the scalars, which are cheap. ESPC's
tile tier is 89% of its repository's bytes — two forecast leads across five
depths — against 44-58 MB for a 2-D scalar field. Splitting gives each half
its own gigabyte.

## How it will run

The orchestrator comes from `realtime-data-repo`, the fetchers and the
published-file contract from `oceansensing.github.io`, both checked out at run
time. This repository will carry `pipeline/products.toml` and nothing else
executable. There are no commands to give yet.

## Structure

```
PLAN.md         the founding plan and running record
CLAUDE.md       what must not be got wrong, and the shared doc doctrine
DECISIONS.md    dated one-way decisions, D1 onward
pipeline/       products.toml — not written yet
```
