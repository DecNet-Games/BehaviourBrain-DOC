---
layout: default
title: Quick Start
nav_order: 4
description: "The shortest path from import to a running, replayed behaviour graph: Home window, setup check, the GuardPatrol sample's state change, and the same run replayed tick by tick."
---

# Quick Start
{: .no_toc }

This is the shortest path from import to a running, replayed behaviour graph. Follow the steps in order; there are no detours and nothing to set up by hand. You finish by replaying the sample's run tick by tick.

## 1. Import the package

Import Behaviour Brain Suite into your project with the Package Manager. That is the whole step: you create nothing — no folders, no assets, no references.

## 2. Open the Home window

Open **Tools / Decnet Games / Behaviour Brain Suite / Home**. The first open shows the welcome screen: the product name, the outcome line, the version line, and four quick actions.

![The Home window's welcome screen on first open]({{ site.baseurl }}/assets/images/home-welcome.png)

## 3. Run Setup / Verify Install

Click **Run Setup / Verify Install**. The window checks the install, creates the working folder `Assets/Behaviour Brain Suite/` (with its `Graphs/` and `Traces/` subfolders) if it is missing, offers the samples import inside the same flow, and reports a plain-language result with a check line per item. If an item cannot complete, the report names that item and the one action that fixes it; the other items still complete. Run it a second time at any point to verify the install again — the end state is the same.

## 4. Open the GuardPatrol sample

Click **Open Samples** and open **GuardPatrol**. If the samples are not imported yet, the import is offered inline — never a file path to hunt for. The scene holds a guard, four waypoints, and a simple intruder trigger on a plain ground plane, with an on-screen readout showing the guard's current state. The visuals are placeholders, labelled as a sample.

## 5. Run the sample and watch the state change

Press **Play**. The guard walks its waypoints. When the intruder enters the yard, the readout changes **Patrol → Chase**; when the intruder leaves, it changes **Chase → Investigate**, and after a short look-around it returns to **Patrol**. The behaviour graph is running, visibly. This state change is the first success: a state machine and behaviour trees running as one graph over one blackboard — in this sample the state machine is the root, and behaviour-tree sequences run inside its states.

![GuardPatrol in Play mode: the guard on its route, the readout showing its current state]({{ site.baseurl }}/assets/images/guard-patrol-scene.png)

![The moment the state changes: the intruder has entered the yard and the readout reads Chase]({{ site.baseurl }}/assets/images/guard-chase-scene.png)

## 6. Replay the run

GuardPatrol ships with a recorded run of this exact scenario. Back in the **Home** window's **Replay** section, click **Open a Recorded Trace** and choose the sample's `GuardPatrol.dectrace` file. The Replay Debugger opens on that run: scrub the timeline and each tick shows the active nodes, the blackboard values, and the score tables, including the exact tick the state changed. You have now seen the run twice — once in the scene, once under the scrubber.

## 7. Second sample: UtilityEnemy (optional)

From **Open Samples**, open **UtilityEnemy**. The enemy chooses between Melee, Ranged and Flee by score: its on-screen readout shows the current choice, and the choice changes as the scores change. Its recorded trace shows the full score table for every tick, including how ties are settled.

![UtilityEnemy running: the readout shows the choice the scores produced]({{ site.baseurl }}/assets/images/utility-scene.png)

## Where to go next

- **Node reference** — every node type in the package, with its tick behaviour and status returns.
- **Blackboard reference** — typed keys, scopes, listeners and snapshots.
- **Trace format reference** — the `.dectrace` file format, recording, and replay in code.
- **Migration notes** — graph files, format versions and the 1.0 to 1.1 upgrade.
- **What v1 is not** — the deliberate limits of this version, stated plainly.

![The shipped quick-start document inside the package — the same path, in the package's own documentation set]({{ site.baseurl }}/assets/images/quick-start-doc.png)
