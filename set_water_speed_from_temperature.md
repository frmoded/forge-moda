---
type: action
inputs: [state, temperature]
description: Set every water particle's speed to the value returned by speed_for_temperature(temperature); ink left alone.
---

# English

For every water particle in `state`, set its `speed` to the value returned by [[speed_for_temperature]] for the given `temperature`. Ink particles are unaffected — speed of ink particles is left as-is. Positions, headings, masses, and types are unchanged. Returns the updated `ParticleState` with `tick`, `width`, and `height` carried through unchanged.

Use vectorized numpy: stack the per-particle fields into arrays, build a boolean mask of water particles (`types == 'water'`), assign the new speed via indexing on that mask, and rebuild the `Particle` list from the resulting arrays. No Python for-loops for the speed assignment.

# Python

```python
def compute(context, state, temperature):
    particles = state.particles
    if not particles:
        return ParticleState(tick=state.tick, particles=[], width=state.width, height=state.height)

    new_speed = context.compute("speed_for_temperature", temperature=temperature)

    ids = numpy.array([p.id for p in particles])
    types = numpy.array([p.type for p in particles])
    xs = numpy.array([p.x for p in particles], dtype=float)
    ys = numpy.array([p.y for p in particles], dtype=float)
    headings = numpy.array([p.heading for p in particles], dtype=float)
    speeds = numpy.array([p.speed for p in particles], dtype=float)
    masses = [p.mass for p in particles]

    is_water = types == 'water'
    speeds[is_water] = new_speed

    updated = [
        Particle(
            id=int(ids[i]),
            type=str(types[i]),
            x=float(xs[i]),
            y=float(ys[i]),
            heading=float(headings[i]),
            speed=float(speeds[i]),
            mass=masses[i],
        )
        for i in range(len(particles))
    ]

    return ParticleState(tick=state.tick, particles=updated, width=state.width, height=state.height)
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[speed_for_temperature]]
