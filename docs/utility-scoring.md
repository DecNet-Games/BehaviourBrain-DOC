---
layout: default
title: Utility Scoring
nav_order: 8
description: "UtilitySelector scores its children with linear, curve and boolean-gate scorers that read blackboard inputs only; the highest score runs, ties settle one documented way, and the full score table is logged every tick."
---

# Utility Scoring, As Nodes
{: .no_toc }

Utility scoring lives inside the same tree as everything else: a `UtilitySelectorNode` is a node in your behaviour tree, not a separate decision system.

## How a tick works

- The `UtilitySelector` **scores its children with scorer leaves and runs the highest score**.
- **Scorer types:** linear, curve and boolean-gate. Weights are documented on the node reference page.
- **Scorers read blackboard inputs only.** Your code feeds the blackboard — distances, health, targets, flags — and the scorers read those keys. Sensing your game world stays in your code, where it already lives.
- **Ties break one documented way: the lowest child index wins.**
- **The full score table is written to the event log on every tick** (`utility` category: `{name} scores {child}={score},... winner {child}`). Scoring is visible data, not a hidden calculation — and because the log is what a `.dectrace` records, you can inspect any tick's scores later in the [Replay Debugger]({{ site.baseurl }}/docs/replay-debugger/).

## In the samples

**UtilityEnemy** chooses between **Melee, Ranged or Flee** from live utility scores. Its on-screen readout shows the current choice, and the choice changes as the scores change. Its shipped recorded trace shows the full score table for every tick, including how ties are settled.

![The UtilityEnemy trace under the scrubber: the score table for one tick, the winner highlighted]({{ site.baseurl }}/assets/images/replay-utility-scores.png)

![UtilityEnemy running in the scene: the readout shows the choice the scores produced]({{ site.baseurl }}/assets/images/utility-scene.png)

## Determinism

Scorers read the same board and produce the same table — the same tree, seed and starting blackboard always produce the same choices. See [Determinism & Seeded Random]({{ site.baseurl }}/docs/determinism/).
