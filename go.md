---
type: action
inputs: [state, dt]
description: Advance the world by one time step. Returns the updated ParticleState.
---

# English

Advance the world by one time step `dt`. Move all particles forward in their current direction by their current speed by calling [[move_all_particles]] with `state` and `dt`. Returns the updated `ParticleState`.

Wall collisions and particle-particle interactions are NOT implemented at this phase — particles may leave the canvas; that is expected for Phase 2.

# Python

```python
def compute(context, state, dt):
    return context.compute("move_all_particles", state=state, dt=dt)
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[move_all_particles]]
