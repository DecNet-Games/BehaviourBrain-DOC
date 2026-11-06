---
layout: default
title: Trace Format
nav_order: 14
description: "The .dectrace format reference: recording a run with TraceRecorder, the plain-text file grammar, and all-or-nothing loading with named errors."
---

# Trace Format Reference — `.dectrace`
{: .no_toc }

A `.dectrace` file is the recording of one run: the executor's events and the blackboard state, tick by tick. The [Replay Debugger]({{ site.baseurl }}/docs/replay-debugger/) opens these files; your own code can record, load and inspect them through the types on this page.

The format is plain text, one record per line, so traces diff cleanly in version control. A trace never contains absolute machine paths. The current format version is **1** (`RecordedTrace.CurrentFormatVersion`).

## Recording a run

Recording is opt-in and read-only. `TraceRecorder` attaches to one `BehaviourExecutor` for one run and only reads the executor's log and the board's snapshot — it never writes to either, so a recorded run's event log is the same as an unrecorded run's, byte for byte.

```csharp
var recorder = new TraceRecorder(executor, seed, "GuardPatrol");
recorder.Begin();
while (running)
{
    executor.Tick();
    recorder.CaptureTick();
}
recorder.SaveToFile(path);          // or recorder.ToTraceText()
```

- Create the recorder with the executor, the seed the run uses, and a tree identity (the name recorded in the header).
- Call `Begin()` before the first tick, then `CaptureTick()` once after each `executor.Tick()`. The calls are checked: a capture that does not line up with the executor's tick counter, or a log cleared mid-run, fails loudly instead of producing a trace that does not match the run.
- `CapturedTicks` reports how many ticks have been captured so far.
- Recorded board values are the board's own snapshot text: scalar values are recorded exactly; a GameObject value is recorded by its object name, which is what the snapshot carries.

## File grammar

```
dectrace 1
seed {int}
tree {text}
scope {text}
ticks {int}
bb0
{key=value lines}
bbend
tick 1
e {event line}
...
bb
{key=value lines}
bbend
...
end ticks={n} events={m}
```

Line by line:

| Line | Meaning |
| --- | --- |
| `dectrace 1` | Magic word and format version; always the first line. |
| `seed {int}` | The executor seed the run was recorded with. |
| `tree {text}` | The tree identity recorded for the run. |
| `scope {text}` | The blackboard scope name the run used. |
| `ticks {int}` | How many tick records follow. |
| `bb0` … `bbend` | The initial board record: the `key=value` lines of the snapshot taken before tick 1, in first-write order. Empty when nothing had been written. |
| `tick {n}` | Opens the record for tick n. Records appear in tick order, 1 first. |
| `e {line}` | One executor event for this tick (see below), in emission order. |
| `bb` … `bbend` | The board record at the end of the tick, same form as the initial record. |
| `end ticks={n} events={m}` | Footer: the tick record count and the total event count. |

**Event lines.** Each `e` line carries the canonical execution-log line exactly as emitted: `t{n}|{category}|{message}`. The tick stamp inside the line always equals the record's tick, and the category is a single token. The categories the shipped nodes emit are:

| Category | Emitted by | Examples |
| --- | --- | --- |
| `executor` | The executor | `tick begin`, `tick end Success` |
| `node` | Every node | `{name} enter`, `{name} exit {status}` |
| `fsm` | `StateMachineNode` | `{name} {from}->{to}` on a transition |
| `utility` | `UtilitySelectorNode` | `{name} scores {child}={score},... winner {child}` |

## Loading and validation

`RecordedTrace.LoadFromFile(path)` reads a file; `RecordedTrace.Parse(text)` parses text directly. A loaded trace exposes `Seed`, `TreeIdentity`, `ScopeName`, `InitialBoard`, `Ticks` (each with its `Events` and end-of-tick `Board`), `TickCount` and `EventCount`.

Loading is all-or-nothing. Anything structurally wrong raises `TraceFormatException` naming the failing element, and no partial trace is ever returned:

- A first line that is not a dectrace header, a version other than 1, a missing or malformed header field, an unknown record kind, or an event kind that is not a single token.
- A file that ends early — before its footer, or inside a tick record — is reported in the form `trace truncated at tick {n}`, where n is the tick whose record is incomplete (or the final tick when only the footer is missing).

The Replay Debugger follows the same rule: a rejected trace shows the recorded reason and draws nothing.
