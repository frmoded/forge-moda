---
type: action
inputs: [scenario_id]
description: Initialize the simulation for the given scenario; reads the named scenario data snippet and produces the initial ParticleState.
---

# English

Initialize the simulation for the given `scenario_id`. Read the named scenario data snippet via `context.compute(scenario_id)` and extract `water_count`, `ink_count` (default 0), `width`, `height`, and `temperature`.

Call [[create_water_particles]] with `count=water_count`, `width=width`, `height=height`, and `temperature=temperature` — that returns a fresh `ParticleState` containing only water rows, with `tick=0` and the scenario's chamber dimensions.

If the scenario's `ink_count` is greater than 0, thread that state through [[create_ink_particles_random]] with `count=ink_count` so the resulting state has water followed by ink. Otherwise return the water-only state directly.

Returns a `ParticleState`. No Python loops; no `Particle` construction.

# Python

```python
def compute(context, scenario_id):
    scenario = context.compute(scenario_id)
    water_count = scenario["water_count"]
    ink_count = scenario.get("ink_count", 0)
    width = scenario["width"]
    height = scenario["height"]
    temperature = scenario["temperature"]

    state = context.compute("create_water_particles", count=water_count, width=width, height=height, temperature=temperature)

    if ink_count > 0:
        state = context.compute("create_ink_particles_random", state=state, count=ink_count)

    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[create_water_particles]] [[create_ink_particles_random]]
