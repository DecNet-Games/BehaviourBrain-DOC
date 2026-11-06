---
layout: default
title: State Machines
nav_order: 7
description: "StateMachineNode: a finite state machine as one node inside your behaviour tree — states as children, annotated transitions, entry and exit actions, one explicit initial state."
---

# State Machines, As One Node
{: .no_toc }

In Behaviour Brain Suite a state machine is not a second runtime bolted onto the tree — it is a node type, `StateMachineNode`, that lives inside your behaviour tree like any other node.

## The model

- **States are children** of the single node.
- **Transitions are annotated edges** between states, of three kinds: **event**, **condition**, or **utility threshold**.
- **Entry and exit actions are first-class children** — a state's hooks are part of the graph, not hidden code paths.
- **Exactly one entry rule: an explicit initial state.** No hidden default, no guesswork about where a machine starts.

## It behaves like any other node

A `StateMachineNode` nested inside a Sequence behaves like any other composite: the same status propagation, the same tick, the same blackboard. Its transitions are written to the run's event log (`fsm` category lines, in the form `{name} {from}->{to}`), so a recorded run shows every state change with the tick it happened on — see [Trace Format]({{ site.baseurl }}/docs/trace-format/).

## In the samples

In **GuardPatrol**, the state machine is the root of the graph: the guard's **Patrol, Chase, Investigate** states are done by a state-machine node, and behaviour-tree sequences run inside its states. When the intruder enters the yard the state flips to Chase; when the trail goes cold, to Investigate; then back to Patrol. Open the sample's recorded trace in the [Replay Debugger]({{ site.baseurl }}/docs/replay-debugger/) and you can scrub to the exact tick of each change.

![GuardPatrol: the state readout at the Chase moment — a state-machine node inside the tree made this change]({{ site.baseurl }}/assets/images/guard-chase-scene.png)

## On paper

Graphs save as [versioned JSON]({{ site.baseurl }}/docs/graph-json/): state-machine state children appear in the order states were added, and transitions are stored with their kind (`event` or `guard`) and, on event transitions, the event name.
