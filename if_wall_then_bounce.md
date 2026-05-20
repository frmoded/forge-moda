---
type: action
inputs: []
description: "Block 13 — control: if a particle touches a wall, bounce it."
---

# English

Inputs: None

If the current particle is touching a wall (its position is at or past any chamber bound): call bounce_off_wall.

Control block: dispatch only. Call bounce_off_wall once with the whole state; it applies the wall test as a vectorized mask internally and threads the state forward.

# Python

```python
def compute(context, state):
    state = context.compute("bounce_off_wall", state=state)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[bounce_off_wall]]
