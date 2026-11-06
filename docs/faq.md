---
layout: default
title: FAQ
nav_order: 19
description: "Straight answers: what ships in the box, which Unity version is supported, how the replay debugger differs from a live debugger, and what determinism does and does not cover."
---

# FAQ
{: .no_toc }

## Is this three separate systems bundled together?

No. There is one graph, one executor, one tick and one typed blackboard. Behaviour trees are the structure; a state machine is a node type inside the tree; utility scoring is a selector node inside the same tree. Nothing is bolted on and nothing needs keeping in sync.

## Which Unity versions are supported?

Unity **6000.5.4f1** — the version the package is built and gate-tested on. Compatibility with other versions is not claimed.

## Does the Replay Debugger attach to my running game?

No. It replays recorded `.dectrace` runs, tick by tick. It never attaches to a running game, and it is never described as doing so. You record a run first; the recording is what you inspect.

## What does "deterministic" actually guarantee?

The same tree, the same seed and the same starting blackboard produce a byte-identical event log, every time — proven by the EditMode test suite that ships in the package. It is a property of one run on one machine; v1 makes no claim about synchronising behaviour across a network.

## Do the samples count as a game?

No. GuardPatrol and UtilityEnemy are teaching scenes with placeholder visuals, labelled as samples. They exist to demonstrate the graph system — the state-machine node inside a tree, and utility choices driven by visible scores — and each ships with the recorded trace of its own run so you can verify what you watched.

## What happens when something is wrong — a bad key, a damaged trace, an old graph file?

It fails loudly, with a named error, and nothing loads or renders halfway:

- A mistyped blackboard write names the key and the expected type: ``key `speed` not registered as float``.
- A damaged or cut-short trace is rejected with the failing tick named.
- A graph file from an unknown format version is rejected before anything is built; a 1.0-format graph loads with a per-node migration report.

## Are there add-on packs or a higher tier?

No. The package is one complete edition — runtime, editor tooling, samples, test suite and documentation in the one box. See [Editions]({{ site.baseurl }}/docs/editions/).

## Does the package phone home or collect analytics?

No. There is no telemetry, no hosted backend and no usage dashboard in v1.

## Where do I get help?

Email support@decnetgames.com or join the Discord — see [Support]({{ site.baseurl }}/docs/support/). Every review is read and responded to.
