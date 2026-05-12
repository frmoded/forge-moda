---
type: action
inputs: [state, x, y, count]
description: Append count ink particles to the state at position (x, y); ids continue from the current max.
---

# English

Create `count` ink particles at position `(x, y)`. Each particle gets:
- a fresh `id` continuing from the current max id in `state.particles` (incrementing sequentially: `max_id + 1, max_id + 2, ..., max_id + count`; if `state.particles` is empty, start from 0)
- `type = 'ink'`
- `x` and `y` set to the click position
- random `heading` uniformly in `[0, 2*pi)`
- `speed` set to the medium speed constant via [[speed_for_temperature]] with `temperature='medium'`
- `mass = 'medium'`

Returns a new `ParticleState` with the existing particles followed by the new ink particles appended; `tick` unchanged; `width` and `height` unchanged.

Use vectorized numpy for the random heading draw and the id assignment; no Python for-loops for the math.

# Python

```python
def compute(context, state, x, y, count):
    speed = context.compute("speed_for_temperature", temperature='medium')

    existing = state.particles
    max_id = int(numpy.max(numpy.array([p.id for p in existing]))) if existing else -1

    new_ids = numpy.arange(max_id + 1, max_id + 1 + count)
    headings = numpy.random.uniform(0, 2 * math.pi, count)

    new_particles = [
        Particle(id=int(new_ids[i]), type='ink', x=x, y=y,
                 heading=float(headings[i]), speed=speed, mass='medium')
        for i in range(count)
    ]

    return ParticleState(
        tick=state.tick,
        particles=existing + new_particles,
        width=state.width,
        height=state.height,
    )
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[speed_for_temperature]]
