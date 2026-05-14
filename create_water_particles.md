---
type: action
inputs: [count, width, height, temperature]
description: Create count water particles placed randomly within an (0,0)-(width,height) rectangle.
---

# English

Create `count` water particles placed at random positions inside the chamber bounds `(0, 0)–(width, height)`. Build the per-field arrays in one vectorized pass:

- `ids`: `numpy.arange(count, dtype=numpy.int64)` (so ids are `0..count-1`).
- `types`: object array of `'water'` (`numpy.full(count, 'water', dtype=object)`).
- `xs`: `numpy.random.uniform(0, width, count)`.
- `ys`: `numpy.random.uniform(0, height, count)`.
- `headings`: `numpy.random.uniform(0, 2 * math.pi, count)`.
- `speeds`: filled with the scalar returned by [[speed_for_temperature]] for the given `temperature` (`numpy.full(count, speed)`).
- `masses`: object array of `'medium'`.

Return a `ParticleState` with `tick=0`, the assembled arrays, and the given `width` and `height`. Do not iterate; do not construct `Particle` objects.

# Python

```python
def compute(context, count, width, height, temperature):
    speed = context.compute("speed_for_temperature", temperature=temperature)
    ids = numpy.arange(count, dtype=numpy.int64)
    types = numpy.full(count, 'water', dtype=object)
    xs = numpy.random.uniform(0, width, count)
    ys = numpy.random.uniform(0, height, count)
    headings = numpy.random.uniform(0, 2 * math.pi, count)
    speeds = numpy.full(count, speed)
    masses = numpy.full(count, 'medium', dtype=object)
    return ParticleState(
        tick=0,
        ids=ids,
        types=types,
        xs=xs,
        ys=ys,
        headings=headings,
        speeds=speeds,
        masses=masses,
        width=width,
        height=height,
    )
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[speed_for_temperature]]
