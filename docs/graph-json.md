---
layout: default
title: Graphs as JSON
nav_order: 11
description: "Behaviour graphs save as versioned JSON: readable, diffable, friendly to version control, with no machine-specific paths — and strict all-or-nothing loading with named errors."
---

# Graphs as Versioned JSON
{: .no_toc }

Behaviour graphs save as **versioned JSON**: readable, diffable, friendly to version control, with no machine-specific paths inside the file. A graph file stores structure plus parameters — node types, names, parameters and blackboard declarations — never absolute machine paths, and never code.

![A saved behaviour graph as JSON in a text editor: the format version field at the top, no machine paths anywhere]({{ site.baseurl }}/assets/images/graph-json.png)

## The current format: 1.1

A 1.1 file carries, in order: `formatVersion`, `name`, `blackboard`, `root`, `nodes`.

- **Blackboard** entries are `{key, type}` pairs in schema registration order. Type words are `bool`, `int`, `float`, `string`, `Vector3`, `GameObject`.
- **Node ids** are `n1`, `n2`, … in pre-order. State-machine state nodes appear in the order states were added; utility scored children in registration order.
- **Node fields** appear in a fixed order: `id`, `type` (the runtime class name), `name` (the effective display name), `parameters` (omitted when the type stores none; keys sorted), `scorer` (utility scored children only), `state` (state-machine state children only: `{name, entryAction, exitAction}` as hook-presence flags), `transitions` (state machines only: ids `x1`, `x2`, … in registration order, with `from`, `to`, `kind` — `event` or `guard` — and the event name on event transitions), `children` (omitted when empty).
- Floats are written in round-trip format, so saving the same graph produces the same bytes every time: save, load, save again is byte-identical.

## Saving refuses what the format cannot represent

At save time the serializer refuses, with a named `GraphFormatException`: a node instance shared between two parents, a state-machine child that is not a registered state, a utility child without a scorer, or a scorer type outside the shipped three.

## Loading is all-or-nothing

At load time the file is checked before anything is built: ids are unique, references resolve, the graph has no cycles and no unreachable nodes, and each node carries only the fields its kind allows. A graph that cannot be rebuilt exactly as stored raises `GraphFormatException` naming the element, and **nothing is loaded**.

**Rebinding.** Delegates — leaf behaviour, scorers, state entry/exit actions, guards — are code, so the file stores their presence and parameters, not the delegates themselves. At load they are re-supplied through `GraphBindings` (`LeafFactory`, `ScorerFactory`, `EntryActionFactory`, `ExitActionFactory`, `GuardFactory`). A binding that is missing or returns null rejects the load, as does a returned leaf whose type name or node name differs from the stored one. A hook is never dropped silently. The load result (`GraphLoadResult`) carries the rebuilt root and, for upgraded files, the migration report.

## Older and newer files

- **Format 1.0** files load and upgrade in memory to 1.1, with a per-node migration report — see [Migration Notes]({{ site.baseurl }}/docs/migration-notes/).
- **Any other version** is rejected with a named error before any construction: the loader never guesses at a format it does not know.
