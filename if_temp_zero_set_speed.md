---
type: action
inputs: [temperature]
description: "Block 24 — control: if temperature is zero, set water speed zero."
---

# English

Inputs: temperature

If `temperature == "zero"`: call set_speed_zero, passing the current simulation state through to it, and return its result.

Control block: dispatch only. If the temperature does not match, return the state unchanged. When it matches, invoke set_speed_zero with `state=state` (the callee requires the state argument) and return exactly what it returns.

# Python

```python
def compute(context, state, temperature):
    if temperature == "zero":
        return context.compute("set_speed_zero", state=state)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[set_speed_zero]]
