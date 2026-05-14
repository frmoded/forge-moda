---
type: action
inputs:
  - state
  - dt
  - temperature
description: Advance the world by one time step at the given temperature. Update water speeds, move, bounce off walls, detect and resolve particle-particle collisions.
---

# English

Advance the world by one time step `dt` at the given `temperature`. Steps, in order, passing the intermediate `ParticleState` forward at each step:

1. Update water-particle speeds for the current temperature via [[set_water_speed_from_temperature]] with `state` and `temperature`. Ink speeds are left alone.
2. Move all particles forward via [[move_all_particles]] with the temperature-adjusted state and `dt`. This is the leaf that increments `tick`.
3. Reflect any particles that crossed canvas walls via [[bounce_all_particles_off_walls]] on the moved state.
4. Detect particle-particle collisions via [[detect_particle_collisions]] on the bounced state. This returns an `(M, 2)` ndarray of `(i, j)` pairs.
5. Resolve those collisions via [[resolve_particle_collisions]] with the bounced state and the pairs array. This swaps headings within each colliding pair.

Returns the updated `ParticleState`. No Python loops; do not inline the per-leaf math.

# Python

```python
def compute(context, state, dt, temperature):
    state1 = context.compute("set_water_speed_from_temperature", state=state, temperature=temperature)
    state2 = context.compute("move_all_particles", state=state1, dt=dt)
    state3 = context.compute("bounce_all_particles_off_walls", state=state2)
    pairs = context.compute("detect_particle_collisions", state=state3)
    state4 = context.compute("resolve_particle_collisions", state=state3, pairs=pairs)
    return state4
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[set_water_speed_from_temperature]] [[move_all_particles]] [[bounce_all_particles_off_walls]] [[detect_particle_collisions]] [[resolve_particle_collisions]]
