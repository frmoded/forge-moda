---
type: action
inputs: [temperature]
description: "Block 22 — control: if temperature is low, set water speed low."
---

# English

Inputs: temperature

If `temperature == "low"`: call set_speed_low.

Control block: dispatch only. If the temperature does not match, return the state unchanged. Otherwise call set_speed_low once and return its result.

# Python

```python
def compute(context, state, temperature):
    if temperature == "low":
        return context.compute("set_speed_low", state=state)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[set_speed_low]]
