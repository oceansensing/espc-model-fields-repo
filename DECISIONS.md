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

## D2 — 2026-08-31 — Heat content is TCHP to the 26 °C isotherm, and absent where there is none

The owner asked for an upper-ocean heat content layer for **hurricane
intensity forecasting**, which decides the quantity: tropical cyclone heat
potential, ρ·c_p times the integral of (T − 26 °C) from the 26 °C isotherm to
the surface, in kJ/cm².

**A fixed-depth integral was the alternative and it answers a different
question.** 0–700 m is the climate-budget convention and says nothing about a
storm; 0–100 m truncates the Loop Current exactly where it matters. What a
storm can extract is bounded by the water above the temperature at which deep
convection stops paying, which is what makes the isotherm the limit rather
than a depth.

**One-way because it is the shape of every published cell.** A reader codes
against the unit, the reference temperature and the meaning of an absent
value; changing any of the three later is a migration on their side, not
ours. Three things are fixed by this entry:

- the **unit**, kJ/cm², which is what the hurricane literature and NOAA/AOML
  publish;
- the **reference**, 26 °C, and it is in the definition rather than a
  configurable — a heat potential to some other isotherm is not this quantity;
- **absent, not zero**, where the surface is at or below 26 °C. Zero would say
  a storm finds no fuel where the truth is that the question does not apply,
  and it would drag the color ramp across every cold ocean on the map. Absent
  is what "not applicable" looks like.

**And a fourth, which is the one that could have been got wrong silently: a
column still warmer than 26 °C at the bottom of the read publishes NOTHING**,
rather than the partial integral. The partial answer would understate heat
exactly where the ocean is most favorable for intensification — the one
direction a hurricane product must not be quietly wrong in.

**The first production run had exactly one such column**, out of about 11,820
warm cells on the global grid — 0.008%, printed in the run log. A coarse
sample taken before the build found none, which is the bias that sample was
expected to have. The case is handled, counted and reported, and that is what
this decision fixes: **a reported hole rather than a silent underestimate.**
The read depth only tunes how often it fires.

**What is NOT decided here**: the read depth (300 m, tuning, and `PLAN.md`
has the measurement that chose it), the color ramp, and the 0–150 display
range. Those move without costing anybody a migration.

## D3 — 2026-08-31 — Temperature at 30 m is its own product

It comes off the same profile read as the heat content — index 10 of a span
that goes to 300 m either way — so publishing it costs one grid and no extra
request. That is the same argument that moved the currents' second layer from
60 m to 50 m: make the standalone layer a by-product of a read something else
already has to make.

**One-way in the small way a published root always is**: it is in the
contract, declared by exactly one origin, and a reader's saved view can name
it. Recorded rather than left to a commit message because *which depth* is
the part a later reader will want the reason for — 30 m is below the diurnal
skin and most of the wind-mixed layer but still inside the water a storm
overturns, and half of all D26 measured that day sat at 65 m or shallower.

