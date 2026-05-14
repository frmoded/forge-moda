---
type: action
inputs: [state, temperature]
description: Set every water particle's speed to the value returned by speed_for_temperature(temperature); ink left alone.
---

# English

For every water row in `state`, set its `speed` to the value returned by [[speed_for_temperature]] for the given `temperature`. Ink rows are left untouched.

Build a boolean mask `is_water = state.types == 'water'`. Copy `state.speeds` so the caller's array is not mutated, then assign the new scalar speed into the masked positions of the copy. Every other field (`ids`, `types`, `xs`, `ys`, `headings`, `masses`, `tick`, `width`, `height`) is carried through by reference.

Return the updated `ParticleState`. No Python loops; no `Particle` construction.

# Python

```python
def compute(context, state, temperature):
    new_speed = context.compute("speed_for_temperature", temperature=temperature)
    is_water = state.types == 'water'
    new_speeds = state.speeds.copy()
    new_speeds[is_water] = new_speed
    return ParticleState(
        tick=state.tick,
        ids=state.ids,
        types=state.types,
        xs=state.xs,
        ys=state.ys,
        headings=state.headings,
        speeds=new_speeds,
        masses=state.masses,
        width=state.width,
        height=state.height,
    )
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[speed_for_temperature]]
