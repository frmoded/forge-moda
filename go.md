---
type: action
inputs: [state, dt, temperature]
description: "Block 9 — go event. One simulation tick; history-dependent per C8 (reads its own prior snapshot to accumulate)."
---

# English

Inputs: `state` (optional), `dt` (optional), `temperature` (optional)

**History-dependent per constitution C8.** `go` accumulates state across invocations rather than being a pure function of its inputs. Resolution order for the starting state:

1. If `state` is explicitly provided (not None and not empty), use it as given — an explicit input always wins and bypasses the snapshot/fallback chain entirely.
2. Otherwise read the most recent snapshot of `go` itself via `context.read_snapshot()` and continue accumulating from the previous tick's result.
3. Otherwise (no prior snapshot — first call in a fresh vault) fall back to the [[sample_state]] data snippet via `context.compute`.

Defaults when omitted: `state` → None (triggers the snapshot → sample_state fallback chain), `dt` → `1/30`, `temperature` → `"medium"`.

Then advance the simulation by one time step:

1. Call [[ask_all_particles]] with `dt`.
2. Call [[ask_water_particles]] with `temperature`.

Thread the simulation state forward between the two calls and return the final state. The tick counter advances exactly once per `go`, inside [[move]]. Repeated `/compute` calls progress through simulation time.

**Known limitation of the snapshot-default (option A).** `context.read_snapshot()` returns the latest capture in `go`'s *outbound* edge directory — for `go` this works only because `go` is a pass-through whose return value equals its terminal callee's ([[ask_water_particles]]) return. If `go` is ever refactored to post-process the state after the last `context.compute` (e.g. `state.tick += 1; return state`), the snapshot read would lag the true return by that post-processing (one tick). Keep `go` a pass-through, or revisit the engine's snapshot-of-self mechanism. The C8 opt-out applies regardless.

# Python

```python
def compute(context, state=None, dt=1/30, temperature="medium"):
    if state is None or state == "":
        state = context.read_snapshot()
        if state is None:
            state = context.compute("sample_state")
    state = context.compute("ask_all_particles", state=state, dt=dt)
    state = context.compute("ask_water_particles", state=state, temperature=temperature)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[sample_state]] [[ask_all_particles]] [[ask_water_particles]]
