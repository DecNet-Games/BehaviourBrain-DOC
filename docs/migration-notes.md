---
layout: default
title: Migration Notes
nav_order: 12
description: "How graph format versions are handled: 1.0 files upgrade in memory to 1.1 with a human-readable per-node migration report; unknown versions are rejected with a named error and nothing loads."
---

# Migration Notes — Graph Files
{: .no_toc }

Behaviour graphs save as versioned JSON (the serializer lives in the Editor assembly, under `Editor/Graph/`). This page describes the format versions this build reads, what the loader does with each, and what an upgrade reports. For the current format's full layout, see [Graphs as JSON]({{ site.baseurl }}/docs/graph-json/).

## Upgrading from format 1.0

The loader reads 1.0 files and upgrades them in memory to 1.1 before building the graph; the file on disk is not rewritten. The 1.0 layout differed in three ways:

- Blackboard declarations sat under `keys` as `{name, valueType}` pairs, with long type words (`Boolean`, `Int32`, `Single`, `String`, `Vector3`, `GameObject`).
- Nodes carried no `name` field.
- Parameters were stored as a flat text map under `params`.

State objects, transitions and child lists already matched the 1.1 shapes.

![The migration report on opening a 1.0-format graph: what changed, listed node by node]({{ site.baseurl }}/assets/images/migration-report.png)

## The migration report

Every upgrade produces a human-readable report — one line per change, in document order:

1. A blackboard line when declarations were converted, in the form `blackboard declarations converted from the 1.0 layout (N keys)`.
2. One line per affected node, in node order — for example ``node 'n3' (SequenceNode): display name defaulted to 'SequenceNode'`` when a 1.0 node carried no name, or ``node 'n5' (TimeLimitNode): parameter values converted from text (maxTicks)`` listing the keys whose JSON type changed. A scorer change is reported on its child node in the same shape.
3. A closing line: `graph format upgraded from 1.0 to 1.1`.

The report travels with the load result (`GraphMigrationReport`), so the person loading the file sees exactly what changed. An upgraded graph re-serializes byte-identically to the same graph authored directly in 1.1.

## What cannot upgrade

A value the migrator cannot convert rejects the load, naming the element: an unknown type word, a number that does not parse, or curve points stored in the flat 1.0 text map. Nothing is partially loaded.

## Any other version

The version gate runs before any construction. A file whose `formatVersion` is neither `1.0` nor `1.1` is rejected with a named error and nothing is loaded — the loader never guesses at a format it does not know.
