---
layout: default
title: Determinism & Seeded Random
nav_order: 10
description: "Seeded, deterministic execution: the same tree, seed and starting blackboard produce a byte-identical event log, proven by the EditMode test suite that ships in the package."
---

# Determinism & Seeded Random
{: .no_toc }

Determinism is the property the rest of the package is built on — it is what makes a recorded run replayable and a bug reproducible.

## The guarantee

The same tree, the same seed and the same starting blackboard produce the same run — a byte-identical event log, every time. This is not a promise in the marketing copy: it is proven by the EditMode test suite that **ships inside the package**, the same suite the package itself is gate-tested with.

## How it is achieved

- **Seeded random, injected at the executor level.** Nodes that need randomness use the context's seeded generator (`SeededRandom`: `NextUInt`, `NextInt`, `NextFloat`, `NextBool`, with `CaptureState` / `RestoreState`). Node code never calls `UnityEngine.Random` — randomness is an input to the run, not a hidden system source.
- **The tick counter is the only clock.** Nodes read the current tick from their context; no node consults a wall clock, so time is an input too.
- **Fixed orders everywhere.** Blackboard iteration is insertion-ordered, change listeners fire in registration order, score ties break by lowest child index — anywhere a run could fork on ordering, the order is defined and documented.
- **Fixed value formatting.** Blackboard snapshots render values in round-trip formats, so board state compares byte for byte across runs.

## What determinism covers — and what it does not

A run is deterministic **for the same tree, the same seed and the same starting blackboard, on one machine**. Version 1 makes no claim about synchronising behaviour across a network — see [What v1 Is Not]({{ site.baseurl }}/docs/what-v1-is-not/).

One reset subtlety, documented rather than hidden: `BehaviourExecutor.Reset()` rewinds the tick counter, clears the log and resets the tree, but does not rewind the generator. To reproduce an identical log, build a fresh executor with the same seed and run the same inputs.

## Why it matters in practice

- **Replay:** a `.dectrace` recording is meaningful because the run it records is the run that happened — see [Replay Debugger]({{ site.baseurl }}/docs/replay-debugger/).
- **The samples:** each shipped sample's fresh run reproduces its shipped trace exactly — see [Samples]({{ site.baseurl }}/docs/samples/).
- **Debugging:** when a buyer reports a run, the same inputs reproduce it — the event log, the score tables and the board values, tick by tick.
