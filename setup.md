---
type: action
inputs: []
description: Initialize the simulation with hardcoded defaults; produces the initial ParticleState.
---

# English

Initialize the simulation. Create 500 water particles in an 800×600 chamber at medium temperature: call [[create_water_particles]] with `count=500`, `width=800`, `height=600`, `temperature='medium'`. Return the resulting `ParticleState` directly. No scenario lookup; defaults are hardcoded for v1.

# Python

```python
def compute(context):
    state = context.compute("create_water_particles", count=500, width=800, height=600, temperature='medium')
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[create_water_particles]]
