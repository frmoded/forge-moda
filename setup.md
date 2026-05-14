---
type: action
inputs: [scenario_id]
description: Initialize the simulation for the given scenario; reads the named scenario data snippet and produces the initial ParticleState.
---

# English

Initialize the simulation. Read the scenario data via `context.compute(scenario_id)` — the snippet ID is passed through from `/moda/init`. Call [[create_water_particles]] with the scenario's `water_count`, `width`, `height`, and `temperature` to produce the initial water particles. If the scenario's `ink_count` is greater than 0, also call [[create_ink_particles_random]] with that count, threading the water-only `ParticleState` through it so the ink ids continue past the water ids. Return a `ParticleState` with `tick=0`, all created particles, and the scenario's `width` and `height`.

# Python

```python
def compute(context, scenario_id):
    scenario = context.compute(scenario_id)
    water_count = scenario["water_count"]
    ink_count = scenario.get("ink_count", 0)
    width = scenario["width"]
    height = scenario["height"]
    temperature = scenario["temperature"]

    water = context.compute(
        "create_water_particles",
        count=water_count,
        width=width,
        height=height,
        temperature=temperature,
    )
    state = ParticleState(tick=0, particles=water, width=width, height=height)

    if ink_count > 0:
        state = context.compute(
            "create_ink_particles_random",
            state=state,
            count=ink_count,
        )

    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[create_water_particles]] [[create_ink_particles_random]]
