---
type: action
inputs: [state]
description: Detect pairs (i, j) that are within 5 units AND moving toward each other; i < j.
---

# English

Detect particle-particle collisions on `state` and return the list of overlapping `(i, j)` pairs as a numpy ndarray of shape `(M, 2)` and dtype `int64`. Vectorized — no Python loops.

Take `xs = state.xs`, `ys = state.ys`. Build the upper-triangular pair indices: `i_idx, j_idx = numpy.triu_indices(xs.size, k=1)`. Compute pair distances: `dx = xs[j_idx] - xs[i_idx]`; `dy = ys[j_idx] - ys[i_idx]`; `dist_sq = dx * dx + dy * dy`. A pair is a candidate if `dist_sq < 5.0 ** 2`.

Approach-direction filter (Phase-5 fix to avoid stuck pairs in discrete time): compute the relative velocity `dvx = state.speeds[j_idx] * numpy.cos(state.headings[j_idx]) - state.speeds[i_idx] * numpy.cos(state.headings[i_idx])` and similarly `dvy` for sines. A candidate counts only if `dx * dvx + dy * dvy < 0` — the pair's separation is shrinking. Combine both masks and stack the surviving `(i, j)` rows: `numpy.stack([i_idx[mask], j_idx[mask]], axis=1).astype(numpy.int64)`.

If no collisions, return an empty array of shape `(0, 2)` and dtype `int64`. Do not return a list.

# Python

```python
def compute(context, state):
    xs = state.xs
    ys = state.ys

    i_idx, j_idx = numpy.triu_indices(xs.size, k=1)

    dx = xs[j_idx] - xs[i_idx]
    dy = ys[j_idx] - ys[i_idx]
    dist_sq = dx * dx + dy * dy

    proximity_mask = dist_sq < 5.0 ** 2

    dvx = (state.speeds[j_idx] * numpy.cos(state.headings[j_idx])
           - state.speeds[i_idx] * numpy.cos(state.headings[i_idx]))
    dvy = (state.speeds[j_idx] * numpy.sin(state.headings[j_idx])
           - state.speeds[i_idx] * numpy.sin(state.headings[i_idx]))

    approach_mask = (dx * dvx + dy * dvy) < 0

    mask = proximity_mask & approach_mask

    if not numpy.any(mask):
        return numpy.empty((0, 2), dtype=numpy.int64)

    return numpy.stack([i_idx[mask], j_idx[mask]], axis=1).astype(numpy.int64)
```
