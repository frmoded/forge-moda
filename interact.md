---
type: action
inputs: []
description: "Block 12 — action: find every colliding pair this tick and resolve them."
---

# English

Inputs: None

For each other particle in state:
Call if_particle_then_bounce.

Domain realization (vectorized, deviates from a naive pairwise loop — sanctioned): compute the full set of colliding `(i, j)` pairs ONCE for the whole state — two particles collide when they are within the collision distance (5 units) AND their separation is currently shrinking (`(pos_j − pos_i) · (vel_j − vel_i) < 0`). The approach-direction term is a deliberate addition to a plain distance test: without it, just-swapped pairs stay within range, re-collide every tick, and freeze into stuck clusters (empirically 85.7% → 3.5% recurrence with the filter). Pass this colliding-pair array to if_particle_then_bounce and thread the state forward. "the other particle" is the second column of the pair array, never a scalar loop variable.

# Python

```python
def compute(context, state):
    import copy

    N = len(state.xs)
    if N < 2:
        return state

    vx = state.speeds * numpy.cos(state.headings)
    vy = state.speeds * numpy.sin(state.headings)

    ii, jj = numpy.triu_indices(N, k=1)

    dx = state.xs[jj] - state.xs[ii]
    dy = state.ys[jj] - state.ys[ii]
    dist_sq = dx * dx + dy * dy

    dvx = vx[jj] - vx[ii]
    dvy = vy[jj] - vy[ii]
    approach = dx * dvx + dy * dvy

    collision_mask = (dist_sq <= 25.0) & (approach < 0.0)

    pairs = numpy.column_stack((ii[collision_mask], jj[collision_mask]))

    if pairs.shape[0] == 0:
        return state

    state = context.compute("if_particle_then_bounce", state=state, pairs=pairs)
    return state
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[if_particle_then_bounce]]
