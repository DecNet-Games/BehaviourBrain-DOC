---
layout: default
title: Getting Started
nav_order: 2
description: "What Behaviour Brain Suite is, what it needs, and the shortest route into the documentation: import, open the Home window, run a sample, replay its recorded run."
---

# Getting Started
{: .no_toc }

## What this package is
{: .no_toc .text-delta }

Behaviour Brain Suite is a behaviour-graph package for Unity 6. One graph holds three techniques that are usually three separate systems:

- **Behaviour trees** — the deep paradigm: composites, decorators and C# leaf nodes under one tick.
- **State machines as a node type** — a `StateMachineNode` sits inside your tree like any other node; its states are its children.
- **Utility scoring as nodes** — a `UtilitySelector` scores its children with scorer leaves and runs the highest score.

One executor ticks the whole graph. One typed blackboard is the data surface every node shares. Execution is seeded and deterministic: the same tree, the same seed and the same starting blackboard produce the same run, byte for byte.

On top of the runtime, the package ships editor tooling: the **Home window** (graphs, setup report, replay entry points), the **Replay Debugger** (plays recorded `.dectrace` runs back tick by tick — recordings only, it never attaches to a running game), and **two sample scenes** whose fresh runs reproduce their shipped traces exactly.

## Requirements

- **Unity 6000.5.4f1** — the version this package is built and gate-tested on. Other versions are not claimed.
- No render-pipeline dependency: the behaviour runtime is plain C#.
- The package is a UPM package (`com.decnet.behaviour-brain`) with assembly definitions, and the full C# source is included.

## The route through these docs

1. [Installation]({{ site.baseurl }}/docs/installation/) — get the package from the Unity Asset Store and into your project.
2. [Quick Start]({{ site.baseurl }}/docs/quick-start/) — import → Home window → setup → GuardPatrol → watch the state change → replay the run.
3. [The Home Window]({{ site.baseurl }}/docs/home-window/) — the surface you will open every day.
4. The three techniques: [Behaviour Trees]({{ site.baseurl }}/docs/behaviour-trees/), [State Machines]({{ site.baseurl }}/docs/state-machines/), [Utility Scoring]({{ site.baseurl }}/docs/utility-scoring/).
5. The data and file formats: [The Blackboard]({{ site.baseurl }}/docs/blackboard/), [Graphs as JSON]({{ site.baseurl }}/docs/graph-json/), [Trace Format]({{ site.baseurl }}/docs/trace-format/).
6. [Node Reference]({{ site.baseurl }}/docs/node-reference/) — every node type in the package.
7. [What v1 Is Not]({{ site.baseurl }}/docs/what-v1-is-not/) — the scope limits, in writing, before you build on it.
