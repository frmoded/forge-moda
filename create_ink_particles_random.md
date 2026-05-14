---
type: action
inputs: [state, count]
description: Append count ink particles at random positions across the chamber; ids continue from the current max.
---

# English

Append `count` ink particles to `state` at random positions uniformly distributed across the chamber bounds (`0..state.width`, `0..state.height`). Use `numpy.concatenate` on each per-field array — no Python loops, no `Particle` construction.

- new ids: `numpy.arange(max_id + 1, max_id + 1 + count, dtype=numpy.int64)` where `max_id = int(state.ids.max()) if state.ids.size > 0 else -1`.
- new types: `numpy.full(count, 'ink', dtype=object)`.
- new xs: `numpy.random.uniform(0, state.width, count)`.
- new ys: `numpy.random.uniform(0, state.height, count)`.
- new headings: `numpy.random.uniform(0, 2 * math.pi, count)`.
- new speeds: `numpy.full(count, speed)` where `speed` comes from `context.compute("speed_for_temperature", temperature='medium')`.
- new masses: `numpy.full(count, 'medium', dtype=object)`.

Return a new `ParticleState` with the concatenated arrays. `tick`, `width`, and `height` carry through unchanged.

# Python

```python
def compute(context, state, count):
    speed = context.compute("speed_for_temperature", temperature='medium')

    max_id = int(state.ids.max()) if state.ids.size > 0 else -1
    new_ids = numpy.arange(max_id + 1, max_id + 1 + count, dtype=numpy.int64)
    new_types = numpy.full(count, 'ink', dtype=object)
    new_xs = numpy.random.uniform(0, state.width, count)
    new_ys = numpy.random.uniform(0, state.height, count)
    new_headings = numpy.random.uniform(0, 2 * math.pi, count)
    new_speeds = numpy.full(count, speed)
    new_masses = numpy.full(count, 'medium', dtype=object)

    return ParticleState(
        tick=state.tick,
        ids=numpy.concatenate([state.ids, new_ids]),
        types=numpy.concatenate([state.types, new_types]),
        xs=numpy.concatenate([state.xs, new_xs]),
        ys=numpy.concatenate([state.ys, new_ys]),
        headings=numpy.concatenate([state.headings, new_headings]),
        speeds=numpy.concatenate([state.speeds, new_speeds]),
        masses=numpy.concatenate([state.masses, new_masses]),
        width=state.width,
        height=state.height,
    )
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[speed_for_temperature]]
