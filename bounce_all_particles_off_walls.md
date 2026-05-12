---
type: action
inputs: [state]
description: Reflect particles that crossed canvas walls back into the canvas; perfectly elastic.
---

# English

Inspect each particle's position relative to the canvas bounds (read `state.width` and `state.height`).

For any particle whose `x < 0` or `x > width`, reflect its heading across the vertical wall: `new_heading = pi - heading`.

For any particle whose `y < 0` or `y > height`, reflect its heading across the horizontal wall: `new_heading = -heading`.

Clamp the position back inside the bounds (`x = max(0, min(width, x))`, `y = max(0, min(height, y))`) so the particle sits on or just inside the wall.

Speed and mass are unchanged.

Returns a new `ParticleState` with positions and headings updated; `tick` unchanged.

Operate on numpy arrays for detection, reflection, and clamping — do not use Python for-loops.

# Python

```python
def compute(context, state):
    particles = state.particles
    if not particles:
        return ParticleState(tick=state.tick, particles=[], width=state.width, height=state.height)

    ids = numpy.array([p.id for p in particles])
    types = [p.type for p in particles]
    masses = [p.mass for p in particles]
    xs = numpy.array([p.x for p in particles], dtype=float)
    ys = numpy.array([p.y for p in particles], dtype=float)
    headings = numpy.array([p.heading for p in particles], dtype=float)
    speeds = numpy.array([p.speed for p in particles], dtype=float)

    w = state.width
    h = state.height

    hit_x = (xs < 0) | (xs > w)
    hit_y = (ys < 0) | (ys > h)

    headings = numpy.where(hit_x, math.pi - headings, headings)
    headings = numpy.where(hit_y, -headings, headings)
    headings = headings % (2 * math.pi)

    xs = numpy.clip(xs, 0, w)
    ys = numpy.clip(ys, 0, h)

    updated = [
        Particle(id=int(ids[i]), type=types[i], x=float(xs[i]), y=float(ys[i]),
                 heading=float(headings[i]), speed=float(speeds[i]), mass=masses[i])
        for i in range(len(particles))
    ]

    return ParticleState(tick=state.tick, particles=updated, width=state.width, height=state.height)
```
