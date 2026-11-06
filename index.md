---
layout: home
title: Home
nav_order: 1
description: "Behaviour Brain Suite runs NPC behaviour as one graph — behaviour trees, state machines as a node type, and utility scoring — deterministic and seed-for-seed, with a replay debugger that plays recorded runs back tick by tick."
permalink: /
---

# Behaviour Brain Suite
{: .fs-9 }

One graph runs your NPC behaviour — **behaviour trees, state machines and utility scoring as a single system, with runs you can reproduce exactly.**
{: .fs-6 .fw-300 }

[Get started]({{ site.baseurl }}/docs/getting-started/){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[Unity Asset Store](https://assetstore.unity.com/publishers/118262){: .btn .fs-5 .mb-4 .mb-md-0 }

![Behaviour Brain Suite — one graph for behaviour trees, state machines and utility scoring]({{ site.baseurl }}/assets/images/cover.png)

---

## Three techniques, one graph

Behaviour Brain Suite is a behaviour-graph package for **Unity 6000.5.4f1**. Behaviour trees are the deep paradigm. A state machine is one node type inside the tree. Utility is a set of scoring nodes inside the same tree. One executor, one tick, one typed blackboard — no second runtime bolted on, no separate system to keep in sync.

And because execution is seeded and deterministic, the same tree with the same seed and the same starting blackboard produces the same run, every time — byte for byte. The proof is the shipped EditMode test suite, not a promise.

<video controls preload="metadata" style="max-width:100%" src="{{ site.baseurl }}/assets/videos/behaviour-brain-demo.mp4"></video>

*The flagship loop: the GuardPatrol sample runs in the scene — then the same run is replayed under the scrubber in the Replay Debugger.*

## A home window, not a folder hunt

The **Behaviour Brain Home** window (Tools / Decnet Games / Behaviour Brain Suite) puts your graphs, the setup report and the replay entry points in one place, with a welcome screen on first open. **Run Setup / Verify Install** checks the install, prepares the working folder and offers the samples, reporting each check in plain language.

![The Behaviour Brain Home window on first open — the welcome screen]({{ site.baseurl }}/assets/images/home-welcome.png)

![The Home window as a working surface: graphs listed, setup report populated, a trace ready in the Replay section]({{ site.baseurl }}/assets/images/home-working.png)

## Replay any run, tick by tick

Any run can be recorded to a `.dectrace` file: node activations, blackboard writes, scores and state transitions, each stamped with its tick. The **Replay Debugger** plays recorded traces back — scrub the timeline and watch the active node move through the tree in the recorded order, with blackboard values and scores shown per tick. The debugger replays recordings only; it never attaches to a running game.

![Scrubbing the recorded GuardPatrol run: the active node walks the tree while the guard's recorded state reads Chase]({{ site.baseurl }}/assets/images/replay-scrub.gif)

![The full score table for a tick of the UtilityEnemy trace — Melee, Ranged and Flee scored, the winner highlighted]({{ site.baseurl }}/assets/images/replay-utility-scores.png)

## Two samples, ready to run

- **GuardPatrol** — a guard patrols its waypoints, chases an intruder, and investigates when it loses the trail. The state changes are done by a state-machine node — Patrol, Chase, Investigate — inside the patrol tree.
- **UtilityEnemy** — an enemy chooses Melee, Ranged or Flee from live utility scores, and you can watch the choice change as the scores change.

Both samples ship with the recorded trace of their own run — and a fresh run of the shipped tree reproduces that trace exactly.

![GuardPatrol running: the guard walks its waypoints and the on-screen readout shows the current state]({{ site.baseurl }}/assets/images/guard-patrol-scene.png)

## Mistakes are loud, never silent

A mistyped blackboard write fails with a named error — ``key `speed` not registered as float`` — instead of returning a default value and hiding the bug. A damaged or cut-short trace is rejected with the failing tick named. A graph file from an unknown format version is rejected before anything loads. Nothing fails halfway, and nothing fails quietly.

## Graphs are versioned JSON

Graphs save as readable, diffable JSON with no machine-specific paths inside the file — friendly to version control. Opening a graph written in the first format version produces a migration report listing what changed, node by node.

![A saved behaviour graph as JSON: readable, diffable, with the format version at the top]({{ site.baseurl }}/assets/images/graph-json.png)

## Where to go next

- [Getting Started]({{ site.baseurl }}/docs/getting-started/) — the shortest path from import to a running, replayed graph.
- [Node Reference]({{ site.baseurl }}/docs/node-reference/) — every node type, its tick behaviour and its status returns.
- [What v1 Is Not]({{ site.baseurl }}/docs/what-v1-is-not/) — the deliberate limits of this version, stated plainly before you build on it.
- [Support]({{ site.baseurl }}/docs/support/) — email and Discord.
