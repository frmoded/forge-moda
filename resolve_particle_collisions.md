---
type: action
inputs: [state, pairs]
description: Swap headings within each colliding pair. Vectorized via fancy indexing.
---

# English

For each colliding `(i, j)` pair, swap the headings of particles `i` and `j` — the simplified elastic-collision model from MoDa Unit 1 Block 16 (`swap_headings`). Speeds, positions, masses, types, and ids are unchanged.

Implementation:
- Build a `headings` numpy array from `state.particles`.
- Snapshot the current headings before any swap (`old = headings.copy()`) so all pair updates see the same pre-state.
- Apply the swap with fancy indexing in a single vectorized step: `headings[pairs[:, 0]] = old[pairs[:, 1]]` and `headings[pairs[:, 1]] = old[pairs[:, 0]]`.
- Build the updated particle list with the new headings, leaving every other field as in the input state.
- Return a new `ParticleState` with `tick` and `width`/`height` unchanged.

Do NOT use Python for-loops over the pairs. If `pairs` is empty (shape `(0, 2)`), return the state with the same heading array unchanged.

For multi-collision clusters where one particle appears in more than one pair, all pair-swaps are applied; the final heading depends on the order NumPy processes them. This is acceptable at the densities Phase 5 reaches.

# Python

```python
def compute(context, state, pairs):
    particles = state.particles
    headings = numpy.array([p.heading for p in particles])

    if pairs.shape[0] == 0:
        return ParticleState(
            tick=state.tick,
            particles=list(particles),
            width=state.width,
            height=state.height,
        )

    old = headings.copy()
    headings[pairs[:, 0]] = old[pairs[:, 1]]
    headings[pairs[:, 1]] = old[pairs[:, 0]]

    updated = [
        Particle(
            id=p.id,
            type=p.type,
            x=p.x,
            y=p.y,
            heading=float(headings[i]),
            speed=p.speed,
            mass=p.mass,
        )
        for i, p in enumerate(particles)
    ]

    return ParticleState(
        tick=state.tick,
        particles=updated,
        width=state.width,
        height=state.height,
    )
```
