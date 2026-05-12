---
type: action
inputs: [state]
description: Detect pairs (i, j) that are within 5 units AND moving toward each other; i < j.
---

# English

Find every pair of particles that are within the collision threshold of `5.0` units AND are moving toward each other (approaching). The approach filter is critical: in discrete time the per-tick movement (~1.7 units at the default speed) is smaller than the collision threshold, so without filtering by approach direction a colliding pair stays inside the threshold for many ticks after their swap, gets re-detected each tick, and the swap toggles back and forth — trapping the pair in place. The filter ensures that once headings swap, the next tick sees them separating and skips them.

Use vectorized NumPy:
- Build position arrays `xs` and `ys`, plus velocity components `vxs = speeds * cos(headings)`, `vys = speeds * sin(headings)`.
- Extract upper-triangular indices via `numpy.triu_indices(n, k=1)` so each pair appears once with `i < j`.
- Compute pairwise differences for the chosen index pairs: `dx = xs[cols] - xs[rows]`, `dy = ys[cols] - ys[rows]`.
- Compute squared distance `d2 = dx*dx + dy*dy` and the distance mask `close = d2 < 25.0` (threshold squared).
- Compute relative velocity components: `dvx = vxs[cols] - vxs[rows]`, `dvy = vys[cols] - vys[rows]`.
- The dot product `dx*dvx + dy*dvy` is the rate of change of squared distance. Negative means the particles are approaching; non-negative means they're either separating or maintaining distance. The approach mask is `approach = (dx*dvx + dy*dvy) < 0`.
- Combined mask: `mask = close & approach`.
- Return the matching pairs as a 2-column ndarray of shape `(M, 2)` with `int` dtype. If no pairs are colliding, return an empty array with shape `(0, 2)`.

Do NOT use Python for-loops. The pairwise tensor at N=1000 is about 8 MB; that is fine. Spatial partitioning is out of scope.

Returns a 2D ndarray of integer pairs `[[i0, j0], [i1, j1], ...]`.

# Python

```python
def compute(context, state):
    particles = state.particles
    n = len(particles)

    if n < 2:
        return numpy.empty((0, 2), dtype=int)

    xs = numpy.array([p.x for p in particles])
    ys = numpy.array([p.y for p in particles])
    headings = numpy.array([p.heading for p in particles])
    speeds = numpy.array([p.speed for p in particles])

    vxs = speeds * numpy.cos(headings)
    vys = speeds * numpy.sin(headings)

    rows, cols = numpy.triu_indices(n, k=1)

    dx = xs[cols] - xs[rows]
    dy = ys[cols] - ys[rows]
    d2 = dx * dx + dy * dy
    close = d2 < 25.0

    dvx = vxs[cols] - vxs[rows]
    dvy = vys[cols] - vys[rows]
    dot = dx * dvx + dy * dvy
    approach = dot < 0.0

    mask = close & approach

    matched_rows = rows[mask]
    matched_cols = cols[mask]

    pairs = numpy.stack([matched_rows, matched_cols], axis=1).astype(int)
    return pairs
```
