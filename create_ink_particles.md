---
type: action
inputs: [x, y]
description: "Block 6 — create 50 ink particles at the click position with random headings."
---

# English

Inputs: x, y

Create 50 ink particles at position `(x, y)`.
Each particle gets a random heading. All 50 particles in one click share a single randomly-drawn heading (so the drop emerges as a coherent puff, not a radial starburst), and each gets its own small random initial speed in `[0, 10)`.

Ink particles are appended to the simulation state; ids continue sequentially from the current maximum id. Mass is set by set_ink_mass; leave it at a 'medium' placeholder here.

# Python

```python
def compute(context, state, x, y):
    count = 50
    max_id = state.ids.max() if len(state.ids) > 0 else -1
    new_ids = numpy.arange(max_id + 1, max_id + 1 + count)
    new_types = numpy.full(count, 'ink', dtype=object)
    new_xs = numpy.full(count, float(x))
    new_ys = numpy.full(count, float(y))
    shared_heading = random.uniform(0, 2 * math.pi)
    new_headings = numpy.full(count, shared_heading)
    new_speeds = numpy.random.uniform(0, 10, count)
    new_masses = numpy.full(count, 'medium', dtype=object)

    ids = numpy.concatenate([state.ids, new_ids])
    types = numpy.concatenate([state.types, new_types])
    xs = numpy.concatenate([state.xs, new_xs])
    ys = numpy.concatenate([state.ys, new_ys])
    headings = numpy.concatenate([state.headings, new_headings])
    speeds = numpy.concatenate([state.speeds, new_speeds])
    masses = numpy.concatenate([state.masses, new_masses])

    return ParticleState(
        tick=state.tick,
        ids=ids,
        types=types,
        xs=xs,
        ys=ys,
        headings=headings,
        speeds=speeds,
        masses=masses,
        width=state.width,
        height=state.height,
    )
```
