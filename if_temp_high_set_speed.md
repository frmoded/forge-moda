---
type: action
inputs: [temperature]
description: "Block 18 — control: if temperature is high, set water speed high."
---

# English

Inputs: `temperature`

Steps:
1. If `temperature == "high"`: call [[set_speed_high]].

Control block: dispatch only. If the temperature does not match, return the state unchanged. Otherwise call [[set_speed_high]] once and return its result.

# Python

```python
def compute(context, state, temperature):
    if temperature == "high":
        return context.compute("set_speed_high", state=state)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[set_speed_high]]
