---
layout: default
title: Node Reference
nav_order: 16
description: "Every node type in the Behaviour Brain Suite runtime: the shared executor model, base classes, composites, the state-machine and utility-selector nodes, eight decorators, and the action and condition leaves."
---

# Node Reference
{: .no_toc }

Every node type in the Behaviour Brain Suite Runtime assembly (`Decnet.BehaviourBrain`). This page explains the model all of them share, then lists each type with its rule. The package itself ships a reference page for every node type in its documentation set; this page is the map of that set.

## The shared model

- **One executor, one tick.** A `BehaviourExecutor` owns one run of one graph. Each `Tick()` advances the whole graph by exactly one pass from the root. The tick counter starts at zero, is incremented before the root is ticked, and is the only source of time available to nodes — no node consults a clock. `RunUntilComplete(maxTicks)` ticks until the root reports a terminal status or the budget is used up.
- **Statuses.** Every tick of every node returns a `NodeStatus`: **Success** or **Failure** (terminal — the activation ends) or **Running** (the node must be ticked again). Running propagates up the tree, and the next tick resumes from the running node — there is no implicit per-frame re-evaluation of finished branches.
- **The context.** Nodes receive a `BehaviourContext` carrying the run's `Board` (blackboard scope), `Random` (the seeded generator), `Log` (the execution log) and `Tick` (the current tick number).
- **The log.** Every run writes an `ExecutionLog`: canonical lines of the form `t{tick}|{category}|{message}` in emission order — node entries and exits, state transitions, utility score tables. This log is what a `.dectrace` records (see [Trace Format]({{ site.baseurl }}/docs/trace-format/)).
- **Determinism.** The same tree, the same seed and the same starting blackboard produce the same log, byte for byte. Nodes that need randomness use `ctx.Random` (`SeededRandom`: `NextUInt`, `NextInt`, `NextFloat`, `NextBool`, with `CaptureState` / `RestoreState`) — never a system random source.
- **Reset.** `Reset()` restores a node and its descendants to their starting state. Note that `BehaviourExecutor.Reset()` rewinds the tick counter, clears the log and resets the tree, but does not rewind the generator; to reproduce an identical log, build a fresh executor with the same seed and run the same inputs.
- **Names.** A node's display name is used in log lines. Where a constructor name is null or empty, the runtime type name is used instead.

## Base classes

| Type | Role |
| --- | --- |
| `BehaviourNode` | Base class for every node: the enter / tick / exit lifecycle. |
| `CompositeNode` | Base class for nodes that own an ordered list of children. |
| `DecoratorNode` | Base class for nodes that wrap exactly one child. |
| `BehaviourAction` | Base class for authored action nodes written as C# classes. |
| `BehaviourCondition` | Base class for authored condition nodes written as C# classes. |

## Composites

| Type | Rule |
| --- | --- |
| `SequenceNode` | Children in order; succeeds when every child succeeds. |
| `SelectorNode` | Children in order; succeeds when any child succeeds. |
| `ParallelNode` | All children ticked together; separate Success and Failure policies combine the results. |
| `StateMachineNode` | A finite state machine as one node: states as children, guard and event transitions. |
| `UtilitySelectorNode` | Every child scored every tick; the highest score runs. Scorers are documented with this type. |

## Decorators

| Type | Rule |
| --- | --- |
| `InverterNode` | Swaps the child's Success and Failure. |
| `SucceederNode` | Turns the child's Failure into Success. |
| `FailerNode` | Turns the child's Success into Failure. |
| `RepeaterNode` | Repeats the child a fixed number of times (or without end). |
| `UntilSuccessNode` | Repeats the child until it succeeds. |
| `UntilFailureNode` | Repeats the child until it fails. |
| `TimeLimitNode` | Fails when the activation consumes its tick budget. |
| `CooldownNode` | Blocks the child for a tick count after each terminal result. |

## Leaves

| Type | Rule |
| --- | --- |
| `ActionNode` | Returns the status a delegate produces. |
| `ConditionNode` | Success when a delegate predicate is true; Failure otherwise. |

## Related references

- [The Blackboard]({{ site.baseurl }}/docs/blackboard/) — the typed key registry, scopes, listeners and snapshots every node shares.
- [Trace Format]({{ site.baseurl }}/docs/trace-format/) — recording runs to `.dectrace`, the file grammar, and replay in code.
- [Migration Notes]({{ site.baseurl }}/docs/migration-notes/) — graph JSON format versions and the 1.0 to 1.1 upgrade.
