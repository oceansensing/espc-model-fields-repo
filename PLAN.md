# espc-model-fields-repo — the founding plan and running record

The ESPC **scalar** fields — the cheap half of the model. Started 2026-08-30, when the repository was created. **Nothing
is built.**

## What it is for

SST, SSS, SSH, sea-ice concentration and thickness, and eventually an
upper-ocean heat content layer. **Five roots today**: `sst-navy.json`,
`sss-navy.json`, `ssh-navy.json`, `sic-navy.json`, `sit-navy.json`.

**They are still in `realtime-data-repo` and have not moved.** This
repository exists so the move has a destination; the migration is a separate
sitting and nothing here is wired yet.

## Why it is its own repository

**Every model splits two ways** (decided 2026-08-30, `espc-model-repo`'s
`DECISIONS.md` D2): `<model>-model-currents-repo` for the tiled vector fields
and `<model>-model-fields-repo` for the scalars. The axis is what costs bytes,
measured rather than chosen — ESPC's tile tier is **89% of its repository**,
two leads across five depths, against 44-58 MB for a 2-D scalar field.

**`espc-model-repo` is the one exception to the naming**, kept knowingly: it
is the ESPC currents repository, and its URL is a live origin that a rename
would 404. Read it as `espc-model-currents-repo`.

## Storage

150.3 MB for the five (**14.7%** of the 1 GB Pages cap), or 195.3 MB
(19.1%) once a heat-content layer joins them — computed 2026-08-30 from
`espc-model-repo`'s own byte log. They could not move into that repository
because currents plus scalars is 982 MB, **96%**, less than one current frame
of margin. That is the whole reason this repository exists.

## Open

1. Everything. No product is defined and nothing is wired.
2. `pipeline/products.toml`, which must declare only roots the site's
   `test-schema.mjs --roots` publishes.
3. Its cron, offset from the siblings so two repositories never read the same
   upstream in the same minute.
