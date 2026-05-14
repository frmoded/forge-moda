---
type: action
inputs: [state, x, y]
description: Mouse-click handler. Drops 50 ink particles at (x, y).
---

# English

Handle a mouse-click event in the simulation area. Drop 50 ink particles at the click position `(x, y)` by calling [[create_ink_particles_at_position]] with `state=state, x=x, y=y, count=50`.

Returns the updated `ParticleState` produced by that call — existing water and ink particles in place, with 50 new ink rows appended at the click position. No Python loops; no `Particle` construction.

# Python

```python
def compute(context, state, x, y):
    return context.compute("create_ink_particles_at_position", state=state, x=x, y=y, count=50)
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[create_ink_particles_at_position]]
