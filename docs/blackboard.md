---
layout: default
title: The Blackboard
nav_order: 9
description: "One typed data surface for the whole graph: bool, int, float, string, Vector3 and object-handle keys, per-agent sub-scopes, deterministic change listeners, and loud named errors instead of silent defaults."
---

# The Blackboard
{: .no_toc }

The blackboard is the data surface every node in a run shares. Nodes read their inputs from it and write their results to it; utility scorers read it and nothing else, so the same board contents always produce the same behaviour.

Two pieces work together:

- **`BlackboardSchema`** — the typed key registry. It declares which keys exist and the value type of each key. One schema is shared by every scope that uses it.
- **`Blackboard`** — one scope of values, built on a schema. Values, write order and change listeners belong to the scope, never to the schema, so two scopes over the same schema — two agents sharing one tree — never see each other's values.

## Registering keys

Register every key on the schema before use, with the method that matches its type:

| Registration | Value type |
| --- | --- |
| `RegisterBool(name)` | bool |
| `RegisterInt(name)` | int |
| `RegisterFloat(name)` | float |
| `RegisterString(name)` | string |
| `RegisterVector3(name)` | Vector3 |
| `RegisterGameObject(name)` | GameObject |

Rules:

- A key name cannot be null, empty, or whitespace (`ArgumentException`).
- Registering the same name with the same type again is a no-op.
- Registering the same name with a different type throws `InvalidOperationException` and names both types, in the form: ``key `speed` is already registered as float and cannot be registered as int``.
- Registration order is preserved: `KeyNamesInOrder` returns the names in the order they were registered, and serialized graphs store them in that order.
- `IsRegistered(name)` answers whether a name is registered; `GetKeyType(name)` returns its registered type and throws `InvalidOperationException` for an unregistered name.

## Creating a scope

```csharp
var board = new Blackboard(schema, "guard-1");
```

The scope name identifies this agent's board in traces and snapshots (`ScopeName`). Each agent gets its own `Blackboard` instance over the shared schema.

## Reading and writing values

Each type has a matching pair: `SetBool` / `GetBool`, `SetInt` / `GetInt`, `SetFloat` / `GetFloat`, `SetString` / `GetString`, `SetVector3` / `GetVector3`, `SetGameObject` / `GetGameObject`.

- A write stores the value and then notifies the key's listeners.
- A read of a registered key that has never been written returns the type's empty value: `false`, `0`, `0f`, `null` (string and GameObject), or `default(Vector3)`. `HasValue(name)` tells you whether the key has been written at least once in this scope.

## Errors are loud and named

There is no silent cast and no substituted default. Accessing a key the schema does not know, or with the wrong type, throws `InvalidOperationException` immediately:

- Unregistered key: ``key `speed` is not registered``
- Registered under another type: ``key `speed` not registered as float``

Fix the registration or the call — the error names the key and the type involved.

![A mistyped blackboard write rejected with its named error — the key and the expected type, as the primary message]({{ site.baseurl }}/assets/images/blackboard-error.png)

## Change listeners

- `AddListener(keyName, listener)` registers a callback that receives the key name. Listeners run synchronously, in registration order, after the new value has been stored.
- `RemoveListener(keyName, listener)` removes one registration of that callback.
- Listeners belong to the scope: a listener added to one agent's board never fires for another agent's writes.

## Snapshots

`Snapshot()` renders the board as text: one `name=value` line per written key, in **first-write order** (the order keys were first written in this scope — not registration order, not alphabetical). It returns an empty string when nothing has been written.

Value formatting is fixed, so snapshots compare byte for byte across runs:

| Type | Format |
| --- | --- |
| bool | `true` or `false` |
| int | Plain decimal |
| float | Round-trip format, invariant culture |
| string | The string itself |
| Vector3 | `(x, y, z)`, each component in round-trip format |
| GameObject | The object's name, or `null` |

Snapshots are what the trace recorder stores for each tick, which is why a recorded run shows exactly the values the board held. See [Trace Format]({{ site.baseurl }}/docs/trace-format/).
