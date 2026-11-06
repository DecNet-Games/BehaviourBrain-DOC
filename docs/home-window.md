---
layout: default
title: The Home Window
nav_order: 5
description: "The Behaviour Brain Home window: your graphs, the plain-language setup report and the replay entry points in one place, with a welcome screen on first open."
---

# The Home Window
{: .no_toc }

Open it from **Tools / Decnet Games / Behaviour Brain Suite / Home**. The Home window is the front door of the package: your graphs, the setup report and the replay entry points in one place — not a folder hunt through the package.

## First open: the welcome screen

The first time the window opens in a project, it shows the welcome screen: the product name, the outcome line, the version line, and four quick actions that lead to setup, the samples, a new graph and the replay entry points.

![The Home window on first open — the welcome screen]({{ site.baseurl }}/assets/images/home-welcome.png)

## The working layout

On later opens the window is a working surface:

- **Your Graphs** — the behaviour graphs in the project's working folder, ready to open.
- **Setup report** — the result of the most recent setup check, line by line.
- **Replay** — the entry point to the Replay Debugger: open a recorded trace and scrub it.

![The Home window as a working surface: graphs listed, setup report populated, a trace available in the Replay section]({{ site.baseurl }}/assets/images/home-working.png)

## Run Setup / Verify Install

The setup check is part of the window, not a separate tool. Clicking **Run Setup / Verify Install**:

- checks the install,
- creates the working folder `Assets/Behaviour Brain Suite/` (with its `Graphs/` and `Traces/` subfolders) if it is missing,
- offers the samples import inside the same flow,
- and reports each item in plain language.

If an item cannot complete, the report names that item and the one action that fixes it; the other items still complete. Running setup again at any point verifies the install a second time — the end state is the same.

![The setup report close-up: every check line visible with its plain-language result]({{ site.baseurl }}/assets/images/setup-report.png)

## From here

- Open a sample — see the [Quick Start]({{ site.baseurl }}/docs/quick-start/).
- Open a recorded trace — see [Replay Debugger]({{ site.baseurl }}/docs/replay-debugger/).
