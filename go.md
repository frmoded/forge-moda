---
type: action
inputs:
  - state
  - dt
description: Advance the world by one time step. Move, bounce off walls, detect and resolve particle-particle collisions.
---

# English

Advance the world by one time step `dt`. Steps, in order:

1. Move all particles forward in their current direction by their current speed via [[move_all_particles]] with `state` and `dt`.
2. Reflect any particles that crossed canvas walls via [[bounce_all_particles_off_walls]] on the moved state.
3. Detect particle-particle collisions via [[detect_particle_collisions]] on the bounced state. This returns the list of overlapping `(i, j)` pairs.
4. Resolve those collisions via [[resolve_particle_collisions]] with the bounced state and the pairs list. This swaps headings within each colliding pair.

Returns the updated `ParticleState`. Pass the intermediate state forward at each step; do not inline the collision math.

# Python

```python
def compute(context, state, dt):
    moved = context.compute("move_all_particles", state=state, dt=dt)
    bounced = context.compute("bounce_all_particles_off_walls", state=moved)
    pairs = context.compute("detect_particle_collisions", state=bounced)
    resolved = context.compute("resolve_particle_collisions", state=bounced, pairs=pairs)
    return resolved
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[move_all_particles]] [[bounce_all_particles_off_walls]] [[detect_particle_collisions]] [[resolve_particle_collisions]]
