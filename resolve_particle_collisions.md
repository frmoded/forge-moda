---
type: action
inputs: [state, pairs]
description: Swap headings within each colliding pair. Vectorized via fancy indexing.
---

# English

Swap headings within each colliding pair, given `pairs` (an ndarray of shape `(M, 2)` from [[detect_particle_collisions]]). Vectorized — no Python loops.

If `pairs.size == 0`, return `state` unchanged. Otherwise snapshot `old = state.headings.copy()` so both sides of the swap read pre-update values, then build `new_headings = state.headings.copy()` and fancy-index: `new_headings[pairs[:, 0]] = old[pairs[:, 1]]; new_headings[pairs[:, 1]] = old[pairs[:, 0]]`.

All other fields (`ids`, `types`, `xs`, `ys`, `speeds`, `masses`, `tick`, `width`, `height`) are carried through by reference. Return the updated `ParticleState`.

# Python

```python
def compute(context, state, pairs):
    if pairs.size == 0:
        return state
    old = state.headings.copy()
    new_headings = state.headings.copy()
    new_headings[pairs[:, 0]] = old[pairs[:, 1]]
    new_headings[pairs[:, 1]] = old[pairs[:, 0]]
    return ParticleState(
        tick=state.tick,
        ids=state.ids,
        types=state.types,
        xs=state.xs,
        ys=state.ys,
        headings=new_headings,
        speeds=state.speeds,
        masses=state.masses,
        width=state.width,
        height=state.height,
    )
```
