---
type: action
inputs: [dt]
description: "Block 10 — control: tell every particle to move, wall-bounce, and interact."
---

# English

Inputs: `dt`

Steps:
For each particle in state:
1. Call [[move]] with `dt`.
2. Call [[if_wall_then_bounce]].
3. Call [[interact]].

This is a control/scope block: pure dispatch. It does NOT iterate particles in Python — it calls each peer block once with the whole state and threads the returned state forward. The called blocks do the vectorized per-particle work internally.

# Python

```python
def compute(context, state, dt):
    state = context.compute("move", state=state, dt=dt)
    state = context.compute("if_wall_then_bounce", state=state)
    state = context.compute("interact", state=state)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[move]] [[if_wall_then_bounce]] [[interact]]
