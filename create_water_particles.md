---
type: action
inputs: [count, width, height, temperature]
description: Create count water particles placed randomly within an (0,0)-(width,height) rectangle.
---

# English

Create `count` water particles placed randomly within an (0,0)-(width, height) rectangle. Each particle gets: `id` in 0..count-1; `type='water'`; random `heading` uniformly in [0, 2*pi); `speed` set from the temperature parameter via [[speed_for_temperature]]; `mass='medium'`. Returns a list of `Particle`.

Use numpy to draw the random positions and headings in one vectorized pass; do not loop in pure Python for the random sampling.

# Python

```python
def compute(context, count, width, height, temperature):
    speed = context.compute("speed_for_temperature", temperature=temperature)
    xs = numpy.random.uniform(0, width, count)
    ys = numpy.random.uniform(0, height, count)
    headings = numpy.random.uniform(0, 2 * math.pi, count)
    ids = numpy.arange(count)
    particles = [
        Particle(id=int(ids[i]), type='water', x=float(xs[i]), y=float(ys[i]),
                 heading=float(headings[i]), speed=float(speed), mass='medium')
        for i in range(count)
    ]
    return particles
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[speed_for_temperature]]
