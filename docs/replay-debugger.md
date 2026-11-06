---
layout: default
title: Replay Debugger
nav_order: 13
description: "The Replay Debugger plays recorded .dectrace runs back tick by tick — active node, blackboard values and score tables per tick. Recordings only: it never attaches to a running game."
---

# Replay Debugger
{: .no_toc }

Open it from **Tools / Decnet Games / Behaviour Brain Suite / Replay Debugger**, or from the Home window's Replay section via **Open a Recorded Trace**.

## What it does

Any run can be recorded to a `.dectrace` file: node activations, blackboard writes, scores and state transitions, each stamped with its tick. The Replay Debugger plays those recordings back:

- **Scrub the timeline** and watch the active node move through the tree in the recorded order.
- **Per tick**, the window shows the active nodes, the blackboard values the board held at that tick, and the utility score tables.
- Find the exact tick a state changed, a score flipped, or a key was written — and see the values that caused it.

![Scrubbing the GuardPatrol trace: the guard's recorded state reads Chase at this tick]({{ site.baseurl }}/assets/images/replay-guard-chase.png)

![The replay-scrub loop: the active-node highlight walks the tree in activation order while the state moves Patrol → Chase]({{ site.baseurl }}/assets/images/replay-scrub.gif)

## Recordings only — stated plainly

The debugger replays **recordings**. It never attaches to a running game, never pauses or inspects a live session, and never steps through live code. A run is recorded first (the recorder is a read-only observer — see [Trace Format]({{ site.baseurl }}/docs/trace-format/)), and the recording is what you inspect. Because runs are deterministic, the recording is the run: same events, same values, same order.

## Damaged traces are rejected, loudly

A damaged or cut-short trace is rejected with the failing tick named — in the form `trace truncated at tick N` — and nothing is rendered partially. The window shows the recorded reason and draws nothing.

![A truncated trace rejected: the message names the failing tick, and no partial tree is rendered behind it]({{ site.baseurl }}/assets/images/replay-truncated.png)

## Try it on the samples

Both shipped samples include the recorded trace of their own run. Open `GuardPatrol.dectrace` and scrub to the tick the readout changed from Patrol to Chase; open the UtilityEnemy trace and read the full score table for any tick, including how ties were settled. See [Samples]({{ site.baseurl }}/docs/samples/).
