---
type: action
inputs: [state]
description: Detect every pair (i, j) of particles whose center-to-center distance is below the collision threshold; i < j.
---

# English

Find every pair of particles whose center-to-center distance is less than the collision threshold of `5.0` units (canvas-internal coordinates).

Use vectorized NumPy:
- Build position arrays `xs` and `ys` from `state.particles`.
- Compute the full pairwise distance matrix via broadcasting: differences `dx = xs[:, None] - xs[None, :]` and `dy = ys[:, None] - ys[None, :]`, then `dist = numpy.sqrt(dx*dx + dy*dy)`.
- Apply the distance threshold to produce a boolean mask.
- Extract only the upper-triangular indices (`numpy.triu_indices(n, k=1)`) so each pair appears exactly once with `i < j` and self-pairs are excluded.
- Return the matching pairs as a 2-column ndarray of shape `(M, 2)` with `int` dtype. If no pairs are in contact, return an empty array with shape `(0, 2)`.

Do NOT use Python for-loops. The pairwise matrix at N=1000 is about 8 MB; that is fine. Spatial partitioning is out of scope.

Returns a 2D ndarray of integer pairs `[[i0, j0], [i1, j1], ...]`.

# Python

```python
def compute(context, state):
    particles = state.particles
    n = len(particles)
    if n == 0:
        return numpy.empty((0, 2), dtype=int)

    threshold = 5.0

    xs = numpy.array([p.x for p in particles])
    ys = numpy.array([p.y for p in particles])

    dx = xs[:, None] - xs[None, :]
    dy = ys[:, None] - ys[None, :]
    dist = numpy.sqrt(dx * dx + dy * dy)

    rows, cols = numpy.triu_indices(n, k=1)
    mask = dist[rows, cols] < threshold

    pairs = numpy.stack([rows[mask], cols[mask]], axis=1).astype(int)
    return pairs
```
