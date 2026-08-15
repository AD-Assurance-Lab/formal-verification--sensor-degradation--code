# CLAUDE.md - read this before doing anything

New repository. Sensor degradation and occlusion certificates. Owner: MS CS student 1.
See `README.md` for what this is and why.

## Two rules specific to this repository

**1. The mask is not optional.** A plain pixel norm ball is rejected lab-wide: one large
enough to contain a night image also contains physically impossible images, and is vacuous as
a safety claim. What makes a norm ball defensible here is that it is **confined to a mask**
with a physical story (a droplet, a mud patch, a blockage). Do not drop the mask in the name
of generality; that is how this becomes another vacuous L-infinity result.

**2. Do not quantify over mask position in phase 1.** Location is combinatorial and it will
eat the project. Fixed grid of locations, then a two to four parameter family inside the
mask, exactly the way the fog family was built. Phase 2 adds the bounded residual, and that
is where dimension grows and where SDP-CROWN finally gets an honest test.

## Inherited standing rules

These are measured results from the parent steering study, not preferences. Violating one
silently reproduces a bug that has already cost this lab real time.

- **A result that contradicts a pre-registered expectation is a bug until proven otherwise.**
  It may not be written up as a finding until a written disposition lists the candidate
  causes that were ruled out.
- **Verification verdicts are committed to git before the corresponding closed-loop run.**
  That is what makes a verdict a prediction. Four criteria in the parent study scored 14/14,
  7/8, 8/8 and 10/10 in-sample and then 2/6, 3/7, 6/10 and 2/4 blind. In-sample agreement
  means nothing here. **Read `docs/STATE_OF_PLAY.md` section 0b in the parent repo before
  writing any code**, so you understand why this rule exists before it inconveniences you.
- **Train on the parameterized family, closed-loop test on points from that family's axis,
  and verify over that same interval.** A train and verify family mismatch is one of the two
  causes never ruled out in the study before last.
- **Every closed-loop number is a failure RATE over at least 10 repetitions**, never a single
  run. Report Wilson intervals.
- **Keep a known-bad negative control.** `S_clear` must fail the conditions it never saw.
  That control is what caught the corridor-centring bug.
- **Disturbances apply at full sensor resolution, before crop and downsampling**, never to
  the network input.
- **Certify against the closed-loop tolerance**, not the per-frame corridor, which is about
  3.4x too permissive.
- **Bound with alpha-CROWN plus input-space branch and bound.** Do not vendor `auto_LiRPA`;
  depend on upstream via pip. SDP-CROWN is off by default and only returns as an explicit
  phase 2 experiment.
- **Never trade experimental quality for speed.** No CPU fallback, no lowered simulator
  quality, no cut epochs.

## The CARLA rule that has bitten five times

> **A read or a placement issued next to a write does not see that write.**
> `world.set_weather()`, spectator `set_transform()` and sensor delivery are all applied by
> the simulator on the NEXT TICK. Nothing errors when you get this wrong.

Never read back state you just wrote; construct it. Match sensor frames on the id
`world.tick()` returns, and never swallow a missing frame.

## CARLA is shared. Operating notes.

- Book it. Zach, three students and the Isuzu project all want the same simulator.
- **Relaunch the server before every measurement run.** It leaks about 10.5 GiB over 11 h.
- **Non-default port** on the lab machine. Check before assuming 2000.
- **Long runs must be detached** (`setsid nohup ... &`). Foreground and harness-waited jobs
  get killed.
- **`pkill -f` matches your own command line.** Use bracket patterns or PIDs.
- **`grep` block-buffers into a file.** Use `--line-buffered`, or a healthy run looks stalled.

## Parent repository

Read before writing code:
`formal-verification--automated-driving--code/CLAUDE.md`, then `docs/STATE_OF_PLAY.md`
sections 0, 0b and 0c, then `docs/TRAPS.md` and `docs/CONSTRAINTS.md`.
