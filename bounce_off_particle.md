---
type: action
inputs: [pairs]
description: "Block 16 — action: swap headings within each colliding pair."
---

# English

Inputs: None

Swap headings between the current particle and the other particle.

Operates on the colliding-pair array pairs (an (M, 2) integer array of `(i, j)` index pairs) passed in from the control chain — it is a parameter, never fetched via context.compute. For every pair `(i, j)`: particle i takes particle j's pre-swap heading and vice versa, applied with vectorized fancy indexing on a snapshot copy of the headings (so both sides read pre-swap values). Speed is unchanged, so kinetic energy is conserved exactly. Positions, types, masses, ids, tick unchanged. If pairs is empty, return the state unchanged.

# Python

```python
def compute(context, state, pairs):
    if pairs is None or len(pairs) == 0:
        return state
    headings = state.headings.copy()
    i_indices = pairs[:, 0]
    j_indices = pairs[:, 1]
    headings[i_indices] = state.headings[j_indices]
    headings[j_indices] = state.headings[i_indices]
    return ParticleState(
        tick=state.tick,
        ids=state.ids,
        types=state.types,
        xs=state.xs,
        ys=state.ys,
        headings=headings,
        speeds=state.speeds,
        masses=state.masses,
        width=state.width,
        height=state.height,
    )
```
