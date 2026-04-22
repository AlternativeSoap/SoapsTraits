# Cooldowns & Durations

All time-based values in SoapsTraits — cooldowns, `add_stat` durations, and potion effect lengths — use the same format.

---

## Time Formats

| Format | Meaning | Example |
|--------|---------|---------|
| Plain number | Ticks (20 ticks = 1 second) | `100` = 5 seconds |
| Number + `s` | Seconds | `5s` = 5 seconds |
| Number + `t` | Explicit ticks | `100t` = 5 seconds |

Quick reference:

| Ticks | Time |
|-------|------|
| 20 | 1 second |
| 60 | 3 seconds |
| 100 | 5 seconds |
| 200 | 10 seconds |
| 600 | 30 seconds |
| 1200 | 1 minute |

---

## Effect Cooldowns

A cooldown prevents an effect from firing again until the timer runs out. Each player has their own independent timer.

```yaml
- trigger: damaged
  cooldown: 100    # 5 second cooldown
  actions:
    - reduce_damage: 30%
```

```yaml
- trigger: kill
  cooldown: 5s     # same, written in seconds
  actions:
    - heal: 5
```

A cooldown only starts after the effect **successfully fires** — if conditions fail, the cooldown is not consumed. Cooldowns reset on server restart or plugin reload.

Omit `cooldown` entirely if you want the effect to fire every single time.

---

## Durations (`add_stat` and `apply_potion_effect`)

Both actions take a `duration` field using the same time format:

```yaml
- add_stat:
    defense: 15
    duration: 100      # 5 seconds

- apply_potion_effect:
    type: SPEED
    duration: 3s
    level: 2
```

If you omit `duration` from `add_stat`, the buff lasts until the trait is removed or changed.

---

## Tick Interval

The `tick` trigger fires at a configurable interval set in `config.yml`:

```yaml
settings:
  tick-interval: 20    # 20 ticks = 1 second (default)
```

Use a cooldown on tick effects to limit how often they actually activate:

```yaml
# fires the effect once every 10 seconds, only when below 40% HP
- trigger: tick
  cooldown: 200
  conditions:
    - health_below: 40%
  actions:
    - heal: 2
```

---

## Auto-Save Interval

Player data is saved periodically to disk. Configured in `config.yml`:

```yaml
settings:
  save-interval: 6000    # 6000 ticks = 5 minutes (default)
```

Data is always saved on reload and server shutdown, so this only affects how often it saves in between.

---

> **Next:** [Bridges & Integrations →](Bridges-and-Integrations.md)
