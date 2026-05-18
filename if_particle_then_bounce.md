---
type: action
inputs: [pairs]
description: "Block 15 — control: for colliding pairs, bounce them off each other."
---

# English

Inputs: None

Steps:
1. If the current particle is colliding with the other particle: call [[bounce_off_particle]].

Control block: dispatch only. It RECEIVES the colliding-pair array `pairs` (an (M, 2) integer array of `(i, j)` index pairs) already computed by [[interact]] — the collision predicate was applied when that array was built, so do not recompute it and do not fetch it from anywhere. If `pairs` is non-empty, call [[bounce_off_particle]] passing `state` and `pairs`; otherwise return the state unchanged. No Python loop.

# Python

```python
def compute(context, state, pairs):
    if pairs.shape[0] == 0:
        return state
    return context.compute("bounce_off_particle", state=state, pairs=pairs)
```

# Dependencies

*Synced from Python. Edit the Python and regenerate, or run "Forge: Sync edges" to refresh.*

[[bounce_off_particle]]
