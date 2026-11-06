---
layout: default
title: Troubleshooting
nav_order: 20
description: "The named errors are the troubleshooting system: blackboard type rejections, rejected traces and graph files each name the element at fault and the fix."
---

# Troubleshooting
{: .no_toc }

Behaviour Brain Suite is built so that failures name themselves. Most problems surface as one of the errors below — the message names the key, the tick or the element at fault, and that name is the fix.

## `key \`speed\` is not registered` / `key \`speed\` not registered as float`

A node read or wrote a blackboard key that the schema does not know, or with the wrong type. There is no silent cast and no substituted default — the write fails immediately.

**Fix:** register the key on the `BlackboardSchema` with the matching `Register…` method before use, or correct the call's type. Registering the same name under a different type is itself an error and names both types. See [The Blackboard]({{ site.baseurl }}/docs/blackboard/).

## `trace truncated at tick N`

The `.dectrace` file ends early — inside tick N's record, or before its footer. The Replay Debugger shows the recorded reason and draws nothing; no partial trace is ever returned by the loader either.

**Fix:** re-record the run, or use an intact copy of the trace. The file grammar is in [Trace Format]({{ site.baseurl }}/docs/trace-format/) if you need to inspect a trace by hand.

## A graph file is rejected on load

Loading is all-or-nothing, and the `GraphFormatException` names the element that failed. The common cases:

- **Unknown format version** — the file's `formatVersion` is neither `1.0` nor `1.1`. The loader never guesses at a format it does not know.
- **A value the migrator cannot convert** (1.0 → 1.1 upgrade): an unknown type word, a number that does not parse, or curve points stored in the flat 1.0 text map.
- **Structure checks** — duplicate ids, unresolved references, cycles, unreachable nodes, or fields a node kind does not allow.
- **A missing binding** — delegates are re-supplied at load through `GraphBindings`; a factory that is missing, returns null, or returns a leaf whose type or node name differs from the stored one rejects the load. A hook is never dropped silently.

See [Graphs as JSON]({{ site.baseurl }}/docs/graph-json/) and [Migration Notes]({{ site.baseurl }}/docs/migration-notes/).

## The setup report names an item that did not complete

Run **Run Setup / Verify Install** in the Home window again and read the report: each item reports in plain language, and a failed item names the one action that fixes it. The other items still complete. See [The Home Window]({{ site.baseurl }}/docs/home-window/).

## A run does not reproduce an earlier log

Check the three inputs determinism depends on: the same tree, the same seed, the same starting blackboard. One documented subtlety: `BehaviourExecutor.Reset()` does not rewind the seeded generator — build a fresh executor with the same seed to reproduce an identical log. See [Determinism & Seeded Random]({{ site.baseurl }}/docs/determinism/).

## Still stuck?

Email support@decnetgames.com or ask on Discord — see [Support]({{ site.baseurl }}/docs/support/). Include the exact error text; the named errors are designed to be pasted into a support message as they are.
