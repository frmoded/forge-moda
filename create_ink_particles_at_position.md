---
type: action
inputs: [state, x, y, count]
description: Append count ink particles to the state at position (x, y); ids continue from the current max.
---

# English

Append `count` ink particles to `state` at position `(x, y)`. Use `numpy.concatenate` on each per-field array — no Python loops, no `Particle` construction.

- new ids: `numpy.arange(max_id + 1, max_id + 1 + count, dtype=numpy.int64)` where `max_id = int(state.ids.max()) if state.ids.size > 0 else -1`.
- new types: `numpy.full(count, 'ink', dtype=object)`.
- new xs: `numpy.full(count, float(x))`. new ys: `numpy.full(count, float(y))`.
- new headings: every particle in this click shares **one** heading — draw a single scalar `h = float(numpy.random.uniform(0, 2 * math.pi))` and broadcast it via `numpy.full(count, h, dtype=float)`. (Per-particle random headings would make the ink spray radially from the click point like a starburst; a single shared heading makes the drop emerge as a coherent puff that then disperses naturally via `resolve_particle_collisions` when water particles bump into it.)
- new speeds: `numpy.full(count, speed)` where `speed` comes from `context.compute("speed_for_temperature", temperature='medium')`.
- new masses: `numpy.full(count, 'medium', dtype=object)`.

Return a new `ParticleState` whose arrays are the existing arrays concatenated with the new ones. `tick`, `width`, and `height` carry through unchanged.

# Python

```python
def compute(context, state, x, y, count):
    max_id = int(state.ids.max()) if state.ids.size > 0 else -1
    new_ids = numpy.arange(max_id + 1, max_id + 1 + count, dtype=numpy.int64)
    new_types = numpy.full(count, 'ink', dtype=object)
    new_xs = numpy.full(count, float(x))
    new_ys = numpy.full(count, float(y))
    h = float(numpy.random.uniform(0, 2 * math.pi))
    new_headings = numpy.full(count, h, dtype=float)
    speed = context.compute("speed_for_temperature", temperature='medium')
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
