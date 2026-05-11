---
type: action
inputs: [state, dt]
description: Advance every particle's position by its velocity over the time step dt. Returns a new ParticleState with tick+1.
---

# English

Advance every particle's position by its velocity over the time step `dt`. For each particle:
- `x = x + speed * cos(heading) * dt`
- `y = y + speed * sin(heading) * dt`

Heading and speed are unchanged. Wall collisions are not handled here.

Returns a new `ParticleState` with all particles at their new positions, `tick` incremented by 1, and the same `width` and `height`.

Use numpy vectorized operations: stack the particle fields into arrays, compute the new positions in one pass, then rebuild the particle list. Do not loop in pure Python for the math.

# Python

```python
def compute(context, state, dt):
    particles = state.particles
    if not particles:
        return ParticleState(tick=state.tick + 1, particles=[], width=state.width, height=state.height)

    ids = numpy.array([p.id for p in particles])
    types = [p.type for p in particles]
    xs = numpy.array([p.x for p in particles])
    ys = numpy.array([p.y for p in particles])
    headings = numpy.array([p.heading for p in particles])
    speeds = numpy.array([p.speed for p in particles])
    masses = [p.mass for p in particles]

    new_xs = xs + speeds * numpy.cos(headings) * dt
    new_ys = ys + speeds * numpy.sin(headings) * dt

    new_particles = [
        Particle(
            id=int(ids[i]),
            type=types[i],
            x=float(new_xs[i]),
            y=float(new_ys[i]),
            heading=float(headings[i]),
            speed=float(speeds[i]),
            mass=masses[i],
        )
        for i in range(len(particles))
    ]

    return ParticleState(tick=state.tick + 1, particles=new_particles, width=state.width, height=state.height)
```
