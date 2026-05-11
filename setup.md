---
type: action
description: Initialize the simulation. Reads the default scenario and produces the initial ParticleState.
---

# English

Initialize the simulation. Read the [[default_diffusion]] scenario to get `count`, `width`, `height`, and `temperature`. Call [[create_water_particles]] with those values to produce the initial particle list. Return a `ParticleState` with `tick=0`, those particles, and the scenario's `width` and `height`.

# Python

```python
def compute(context):
    scenario = context.compute("default_diffusion")
    count = scenario.get("count", 500)
    width = scenario.get("width", 800)
    height = scenario.get("height", 600)
    temperature = scenario.get("temperature", "medium")
    particles = context.compute("create_water_particles", count=count, width=width, height=height, temperature=temperature)
    return ParticleState(tick=0, particles=particles, width=width, height=height)
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[default_diffusion]] [[create_water_particles]]
