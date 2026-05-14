---
type: action
inputs: [state, dt]
description: Advance every particle's position by its velocity over the time step dt. Returns a new ParticleState with tick+1.
---

# English

Advance every row's position by velocity over the time step `dt`. Compute the new position arrays in one vectorized pass:

- `new_xs = state.xs + state.speeds * numpy.cos(state.headings) * dt`
- `new_ys = state.ys + state.speeds * numpy.sin(state.headings) * dt`

`headings`, `speeds`, `ids`, `types`, `masses`, `width`, and `height` are carried through by reference. `tick` increments by 1. Wall collisions are not handled here.

Return the updated `ParticleState`. No Python loops; no `Particle` construction.

# Python

```python
def compute(context, state, dt):
    new_xs = state.xs + state.speeds * numpy.cos(state.headings) * dt
    new_ys = state.ys + state.speeds * numpy.sin(state.headings) * dt
    return ParticleState(
        tick=state.tick + 1,
        ids=state.ids,
        types=state.types,
        xs=new_xs,
        ys=new_ys,
        headings=state.headings,
        speeds=state.speeds,
        masses=state.masses,
        width=state.width,
        height=state.height,
    )
```
