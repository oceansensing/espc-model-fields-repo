# Decisions

Dated, irreversible-leaning decisions, one entry each, newest last. The
reasoning lives in `PLAN.md`.

**What counts as one-way in a data repository**: a decision that puts bytes in
readers' hands under a shape they will code against; a decision about which
repository owns a product, since moving one costs a migration in two places;
and a decision that forecloses an upstream.

**Executed 2026-08-31.** D1 was taken the day this repository was created and
the products arrived the next day; the entry below is unchanged, with the
execution recorded at its end. **No new door closed in the migration itself** —
what it produced was a bug and a gate, which are `PLAN.md`'s.

## D1 — 2026-08-30 — Its own repository, under the currents/fields convention

The ESPC **scalar** fields — the cheap half of the model. Created as half of a pair with `espc-model-repo` — which is the ESPC **currents** repository despite its name, under the
convention decided the same day: **every model splits two ways along the axis
that costs bytes.** See `espc-model-repo`'s `DECISIONS.md` D2 for the
measurement and the reasoning.

One-way in the ordinary data-repository sense: moving a product between
repositories is cheap in machinery and expensive in everything that points at
it — roots in the contract, origins in the site's config, and the union
`check:docs` holds across origins.

## Open, and not decided here

Everything else: which variables become products, at what depths, on what
grid, and whether forecast leads are carried.

### Executed 2026-08-31

The five roots moved and this origin published them at 06:26Z — three
products, all fresh, four tile tiers, hour 06:00Z. **What it cost the consumer
was one line** in the site's `MAP_ORIGINS`, which is the claim the origins
design was built to make true and the reason the ice was held back in August
as a live test of it.

**What makes it one-way from today rather than from 2026-08-30**: readers now
hold bundles that route these five roots here, and `realtime-data-repo` has
stopped declaring them. Moving them back would cost a migration in three
repositories, not one.

Measured on arrival: ≈169.5 MB, **16.6%** of the 1 GB cap, against a 150.3 MB
projection that under-counted the regional grids. `PLAN.md` has the table.
