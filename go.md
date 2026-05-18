---
type: action
inputs: [dt, temperature]
description: "Block 9 — go event. One simulation tick: ask all particles, then ask water particles."
---

# English

Inputs: `dt`, `temperature`

Steps:
1. Call [[ask_all_particles]] with `dt`.
2. Call [[ask_water_particles]] with `temperature`.

This is the per-tick event. Thread the simulation state forward between the two calls and return the final state. The tick counter advances exactly once per `go`, inside [[move]].

# Python

```python
def compute(context, state, dt, temperature):
    state = context.compute("ask_all_particles", state=state, dt=dt)
    state = context.compute("ask_water_particles", state=state, temperature=temperature)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[ask_all_particles]] [[ask_water_particles]]
