---
type: action
inputs: [state]
description: Reflect particles that crossed canvas walls back into the canvas; perfectly elastic.
---

# English

Reflect any row whose position crossed a chamber wall. Compute two boolean masks against the chamber bounds (`state.width`, `state.height`):

- `hit_x = (state.xs < 0) | (state.xs > state.width)`
- `hit_y = (state.ys < 0) | (state.ys > state.height)`

Update headings vectorized: first `new_headings = numpy.where(hit_x, math.pi - state.headings, state.headings)`, then `new_headings = numpy.where(hit_y, -new_headings, new_headings)`, then normalize via `new_headings = new_headings % (2 * math.pi)`.

Clamp positions back inside the bounds with `numpy.clip(state.xs, 0, state.width)` and `numpy.clip(state.ys, 0, state.height)`.

`ids`, `types`, `speeds`, `masses`, `tick`, `width`, and `height` are carried through. Return the updated `ParticleState`. No Python loops.

# Python

```python
def compute(context, state):
    hit_x = (state.xs < 0) | (state.xs > state.width)
    hit_y = (state.ys < 0) | (state.ys > state.height)

    new_headings = numpy.where(hit_x, math.pi - state.headings, state.headings)
    new_headings = numpy.where(hit_y, -new_headings, new_headings)
    new_headings = new_headings % (2 * math.pi)

    new_xs = numpy.clip(state.xs, 0, state.width)
    new_ys = numpy.clip(state.ys, 0, state.height)

    return ParticleState(
        tick=state.tick,
        ids=state.ids,
        types=state.types,
        xs=new_xs,
        ys=new_ys,
        headings=new_headings,
        speeds=state.speeds,
        masses=state.masses,
        width=state.width,
        height=state.height,
    )
```
