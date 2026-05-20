---
type: action
role: root
inputs: [state, dt, temperature]
description: "Block 9 — go event. One simulation tick; history-dependent per C8 (reads its own prior snapshot to accumulate)."
generation_notes: |
  Keep go a pass-through (return the last context.compute result
  directly) — do NOT post-process state after the last call. The
  snapshot-default reads go's outbound edge directory, which equals
  the terminal callee's return only while go stays pass-through.
  Any post-processing (e.g. state.tick += 1) would cause
  read_snapshot() to lag the true return by one tick.
---

# English

Inputs: state (optional), dt (optional), temperature (optional)

**History-dependent per constitution C8.** go accumulates state across invocations rather than being a pure function of its inputs. Resolution order for the starting state:

If state is explicitly provided (not None and not empty), use it as given — an explicit input always wins and bypasses the snapshot/fallback chain entirely.
Otherwise read the most recent snapshot of go itself via context.read_snapshot() and continue accumulating from the previous tick's result.
Otherwise (no prior snapshot — first call in a fresh vault) fall back to sample_state.

Defaults when omitted: state → None (triggers the snapshot → sample_state fallback chain), dt → 1/30, temperature → "medium".

Then advance the simulation by one time step:

Call ask_all_particles with dt.
Call ask_water_particles with temperature.

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
