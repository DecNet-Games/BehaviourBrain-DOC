---
layout: default
title: Installation
nav_order: 3
description: "Install Behaviour Brain Suite from the Unity Asset Store into a Unity 6000.5.4f1 project, then let the Home window's setup check verify the install and prepare the working folder."
---

# Installation
{: .no_toc }

## Unity Asset Store only
{: .no_toc .text-delta }

Behaviour Brain Suite is sold and delivered exclusively through the **Unity Asset Store**. There is no other download channel: if you obtained the package anywhere else, it is not the supported build.

- Publisher page: [Decnet Games on the Unity Asset Store](https://assetstore.unity.com/publishers/118262)

## Requirements

- **Unity 6000.5.4f1.** This is the only version the package is compiled and gate-tested on; compatibility with other versions is not claimed.
- A project you can open in the Editor — the package's tooling (Home window, Replay Debugger) is Editor-side; the behaviour runtime itself is plain C# with no render-pipeline dependency.

## Import

1. Open your project in Unity 6000.5.4f1.
2. Download and import Behaviour Brain Suite from the Asset Store in the Package Manager, as with any Asset Store package.
3. That is the whole manual step: you create nothing — no folders, no assets, no references.

The package arrives as a UPM package, `com.decnet.behaviour-brain`, with assembly definitions and the full C# source included. The EditMode test suite ships inside the package as well — it is the same suite behind the determinism record described in [Determinism & Seeded Random]({{ site.baseurl }}/docs/determinism/).

## Verify the install

Open **Tools / Decnet Games / Behaviour Brain Suite / Home** and click **Run Setup / Verify Install**. The window:

- checks the install,
- creates the working folder `Assets/Behaviour Brain Suite/` (with its `Graphs/` and `Traces/` subfolders) if it is missing,
- offers the samples import inside the same flow,
- and reports a plain-language result with a check line per item.

If an item cannot complete, the report names that item and the one action that fixes it; the other items still complete. You can run setup again at any point to verify the install a second time — the end state is the same.

![The setup report immediately after Run Setup / Verify Install: every check line with its plain-language result]({{ site.baseurl }}/assets/images/setup-report.png)

## Next step

Follow the [Quick Start]({{ site.baseurl }}/docs/quick-start/): open the GuardPatrol sample, watch the guard change state, then replay that same run in the Replay Debugger.
