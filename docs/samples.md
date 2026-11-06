---
layout: default
title: Samples
nav_order: 15
description: "The two shipped samples: GuardPatrol, whose state changes are done by a state-machine node inside the patrol tree, and UtilityEnemy, which chooses Melee, Ranged or Flee from live utility scores. Each ships with the recorded trace of its own run."
---

# Samples
{: .no_toc }

Two sample scenes ship with the package, offered from the Home window's setup flow and its **Open Samples** action. The samples are teaching scenes with placeholder visuals, labelled as samples — they demonstrate the graph system, and their behaviour is the point.

## GuardPatrol

A guard patrols its waypoints, chases an intruder, and investigates when it loses the trail. The scene holds a guard, four waypoints, and a simple intruder trigger on a plain ground plane, with an on-screen readout showing the guard's current state.

- The state changes — **Patrol, Chase, Investigate** — are done by a state-machine node inside the patrol tree: in this sample the state machine is the root, and behaviour-tree sequences run inside its states.
- When the intruder enters the yard, the readout changes **Patrol → Chase**; when the intruder leaves, **Chase → Investigate**; after a short look-around it returns to **Patrol**.

![GuardPatrol running: the guard between waypoints, the readout showing Patrol]({{ site.baseurl }}/assets/images/guard-patrol-scene.png)

![The Chase moment: the intruder has entered the yard and the state has changed]({{ site.baseurl }}/assets/images/guard-chase-scene.png)

## UtilityEnemy

An enemy chooses **Melee, Ranged or Flee** from live utility scores. Its on-screen readout shows the current choice, and the choice changes as the scores change — the scores themselves come from scorer leaves reading the blackboard.

![UtilityEnemy running: the readout shows the choice the scores produced]({{ site.baseurl }}/assets/images/utility-scene.png)

## Each sample ships its own recorded run

Both samples ship with the recorded trace of their own run. Open a sample's `.dectrace` in the [Replay Debugger]({{ site.baseurl }}/docs/replay-debugger/) and scrub the run tick by tick: the exact tick the guard changed state, the full score table behind every UtilityEnemy choice, the blackboard values at each step.

And because execution is deterministic, a fresh run of the shipped tree reproduces that shipped trace exactly — the samples are checkable, not just watchable. See [Determinism & Seeded Random]({{ site.baseurl }}/docs/determinism/).
