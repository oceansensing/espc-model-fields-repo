# Decisions

Dated, irreversible-leaning decisions, one entry each, newest last. The
reasoning lives in `PLAN.md`.

**What counts as one-way in a data repository**: a decision that puts bytes in
readers' hands under a shape they will code against; a decision about which
repository owns a product, since moving one costs a migration in two places;
and a decision that forecloses an upstream.

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
