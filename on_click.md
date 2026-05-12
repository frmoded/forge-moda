---
type: action
inputs: [state, x, y]
description: Mouse-click handler. Drops 50 ink particles at (x, y).
---

# English

Handle a mouse-click event in the simulation area. Drop 50 ink particles at the click position `(x, y)` by calling [[create_ink_particles_at_position]] with `count=50`.

Returns the updated `ParticleState` containing both the existing water particles and the newly-added ink particles. Speed and mass for the new ink particles are set by the create leaf, matching MoDa Unit 1 Block 7 (speed = medium) and Block 8 (mass = medium).

# Python

```python
def compute(context, state, x, y):
    updated_state = context.compute("create_ink_particles_at_position", state=state, x=x, y=y, count=50)
    return updated_state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[create_ink_particles_at_position]]
