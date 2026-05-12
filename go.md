---
type: action
inputs:
  - state
  - dt
description: Advance the world by one time step. Returns the updated ParticleState.
---

# English

Advance the world by one time step `dt`. First move all particles forward in their current direction by their current speed (call [[move_all_particles]] with `state` and `dt`). Then reflect any particles that crossed the canvas walls (call [[bounce_all_particles_off_walls]] on the moved state). Returns the updated `ParticleState`.

Particle-particle interactions are NOT implemented at this phase — Phase 5.

# Python

```python
def compute(context, state, dt):
    moved = context.compute("move_all_particles", state=state, dt=dt)
    bounced = context.compute("bounce_all_particles_off_walls", state=moved)
    return bounced
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[move_all_particles]] [[bounce_all_particles_off_walls]]
