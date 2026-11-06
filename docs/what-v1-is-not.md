---
layout: default
title: What v1 Is Not
nav_order: 18
description: "The deliberate limits of Behaviour Brain Suite v1, stated plainly: no sensing or steering, no timeline integration, no multiplayer-authoritative behaviour, no cloud or analytics."
---

# What v1 Is Not
{: .no_toc }

Behaviour Brain Suite v1 is a behaviour-graph package: behaviour trees, state machines as a node type, and utility scoring in one graph, with a recorded-trace replay debugger. Just as deliberately, it is not the following. This page states the limits plainly so you can judge the fit before you build on it.

- Cutscene/sequencer parity chasing (no timeline integration promises).
- Runtime navmesh / sensing beyond utility inputs: scorers read blackboard keys the buyer feeds; no perception, no pathfinding, no steering in v1.
- Multiplayer-authoritative behaviour (no netcode, no server-authoritative determinism claims).
- Cloud/analytics: no telemetry, no hosted backend, no usage dashboards.

What this means in practice:

- Your own code feeds the blackboard — distances, health, targets, flags — and scorers and conditions read those keys. Sensing your game world stays in your code, where it already lives.
- Movement stays in your code too: nodes decide, your scripts act on the decisions.
- A run is deterministic for the same tree, the same seed and the same starting blackboard. That determinism is a property of one run on one machine; v1 makes no claim about synchronising behaviour across a network.

Two more limits worth stating in the same plain terms:

- **The Replay Debugger is not a live debugger.** It replays recorded `.dectrace` runs; it never attaches to a running game.
- **Compatibility is claimed for Unity 6000.5.4f1 only** — the version the package is built and gate-tested on.

For what v1 does include, start with the [Node Reference]({{ site.baseurl }}/docs/node-reference/) and the [Quick Start]({{ site.baseurl }}/docs/quick-start/).
