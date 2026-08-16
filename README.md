# formal-verification--sensor-degradation--code

Formal verification of a driving policy under sensor degradation and occlusion.

**Owner:** MS CS student 1. **Status:** new, empty.

## What this is for

Lens dirt, water droplets, mud, partial blockage, dead pixels, ISP faults, LiDAR return
dropout, certified as an ODD boundary.

The commercial artifact is a number an engineer acts on:

> Certified maximum degradation before the function fails.

which converts directly into a cleaning-system trigger threshold or a fault-tolerance
requirement.

## Why this one

These are the failures fleet operators see daily, camera cleaning is already a Tier 1
product line with a budget attached, and ISO 21448 treats sensor degradation as a canonical
triggering condition. It is also the best expo demo available to us, because a dirty camera
is understood in one sentence and fog is not.

It is fast because it reuses everything: same Town04 lap, same trained students `S_clear`
and `S_mixed`, same verifier, same closed-loop harness. Only the disturbance family changes.

## Formulation, in this order

**Phase 1. Masked and physically parameterized.** A fixed mask (droplet, mud patch,
blockage), and inside it a two to four parameter family (opacity, blur radius, attenuation),
built the way the fog family was built so the verifier still sees a low-dimensional box.

Use a **fixed grid of mask locations**. Do not try to quantify over mask position in phase 1.
Location is combinatorial and it will eat the project.

**Phase 2. A bounded residual.** Add an L-infinity residual inside the mask for content we
refuse to model. This is the first point in the lab's work where perturbation dimension grows
meaningfully, which makes phase 2 the honest test of two open questions:

- whether SDP-CROWN's L2 advantage is real here, or vacuous as it was on low-rank sets
- where verifier cost falls over, which is `formal-verification--verifier-scaling--code`

## The mask is not optional

A plain pixel ball is rejected lab-wide: one large enough to contain a night image also
contains physically impossible images, and is vacuous as a safety claim. The **mask** is what
makes a norm ball defensible here, because the perturbation is confined to a small region
with a physical story. Do not drop it in the name of generality.

## Prior art in this lab

Read before writing code:

- `formal-verification--automated-driving--code/CLAUDE.md`
- `formal-verification--automated-driving--code/docs/STATE_OF_PLAY.md`, sections 0, 0b, 0c
- `formal-verification--automated-driving--code/docs/TRAPS.md` and `docs/CONSTRAINTS.md`
- `lab--future-plans--docs/RESEARCH_DIRECTIONS.md`, entries A2, Q3 and R2

## License

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
