---
layout: default
title: Behaviour Trees
nav_order: 6
description: "The behaviour-tree core: Sequence, Selector and Parallel composites, eight decorators, C# action and condition leaves, and the frozen tick semantics every node shares."
---

# Behaviour Trees
{: .no_toc }

Behaviour trees are the deep paradigm of Behaviour Brain Suite: the tree is the structure everything else plugs into. A state machine is one node type inside the tree, and utility scoring lives in the tree as a selector node — see [State Machines]({{ site.baseurl }}/docs/state-machines/) and [Utility Scoring]({{ site.baseurl }}/docs/utility-scoring/).

## Composites

- **Sequence** — children in order; succeeds when every child succeeds.
- **Selector** — children in order; succeeds when any child succeeds.
- **Parallel** — all children ticked together, with a documented finish policy: separate Success and Failure policies combine the children's results.

## Decorators

Each decorator wraps exactly one child:

- **Inverter** — swaps the child's Success and Failure.
- **Repeater** — repeats the child a fixed number of times (or without end).
- **Succeeder** — turns the child's Failure into Success.
- **Failer** — turns the child's Success into Failure.
- **UntilSuccess** — repeats the child until it succeeds.
- **UntilFailure** — repeats the child until it fails.
- **TimeLimit** — fails when the activation consumes its tick budget.
- **Cooldown** — blocks the child for a tick count after each terminal result.

## Leaves: your code, as nodes

- **Action nodes** and **Condition nodes** are authored as C# classes (`BehaviourAction`, `BehaviourCondition`) and are also available as script-callable hooks (`ActionNode`, `ConditionNode` delegate forms). Your game logic lives in these leaves; the tree decides when they run.
- Nodes decide — movement, sensing and acting on the world stay in your scripts. Scorers and conditions read the blackboard; see [What v1 Is Not]({{ site.baseurl }}/docs/what-v1-is-not/) for the exact split.

## Tick semantics — frozen, and written down

- **One tick is one traversal pass.** A `BehaviourExecutor` owns one run of one graph; each `Tick()` advances the whole graph by exactly one pass from the root.
- Every tick of every node returns a `NodeStatus`: **Success** or **Failure** (terminal — the activation ends) or **Running** (the node must be ticked again).
- **Running propagates up**, and the next tick resumes from the running node. There is no implicit per-frame re-evaluation of finished branches.
- The tick counter is the only source of time available to nodes — no node consults a clock. That is part of what makes a run reproducible; see [Determinism & Seeded Random]({{ site.baseurl }}/docs/determinism/).
- `Reset()` restores a node and its descendants to their starting state.

Every run writes an execution log — node entries and exits, state transitions, utility score tables — and that log is what a `.dectrace` recording stores. See [Trace Format]({{ site.baseurl }}/docs/trace-format/).

## Every type, in detail

The [Node Reference]({{ site.baseurl }}/docs/node-reference/) gives each node type's construction, parameters, tick behaviour and status returns.
