---
type: action
inputs: [temperature]
description: "Block 17 — control: tell water particles to update speed for the temperature."
---

# English

Inputs: temperature

For each water particle in state:
Call if_temp_high_set_speed with temperature.
Call if_temp_medium_set_speed with temperature.
Call if_temp_low_set_speed with temperature.
Call if_temp_zero_set_speed with temperature.

Control block: pure dispatch. Call each peer once with the whole state and thread the state forward. No Python loop; the water mask is applied by the downstream set-speed blocks.

# Python

```python
def compute(context, state, temperature):
    state = context.compute("if_temp_high_set_speed", state=state, temperature=temperature)
    state = context.compute("if_temp_medium_set_speed", state=state, temperature=temperature)
    state = context.compute("if_temp_low_set_speed", state=state, temperature=temperature)
    state = context.compute("if_temp_zero_set_speed", state=state, temperature=temperature)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[if_temp_high_set_speed]] [[if_temp_medium_set_speed]] [[if_temp_low_set_speed]] [[if_temp_zero_set_speed]]
