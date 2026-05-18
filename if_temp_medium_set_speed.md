---
type: action
inputs: [temperature]
description: "Block 20 — control: if temperature is medium, set water speed medium."
---

# English

Inputs: `temperature`

Steps:
1. If `temperature == "medium"`: call [[set_speed_medium]].

Control block: dispatch only. If the temperature does not match, return the state unchanged. Otherwise call [[set_speed_medium]] once and return its result.

# Python

```python
def compute(context, state, temperature):
    if temperature == "medium":
        return context.compute("set_speed_medium", state=state)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[set_speed_medium]]
