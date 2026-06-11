# Conditions

Conditions gate whether an effect runs. **All** conditions in an effect must pass. There is no OR logic within a single effect; chain separate effects if you need alternatives.

## YAML format

Each condition is one entry in a list:

```yaml
conditions:
  - health_below: 40%
  - chance: 25%
  - has_target: true
```

Percent values accept a `%` suffix or plain numbers (`40` and `40%` both work).

## All conditions (1.0.0)

### Health (player)

| Condition | Value | Passes when |
|-----------|-------|-------------|
| `health_below` | `30%` | Player HP% is **strictly below** the value |
| `health_above` | `70%` | Player HP% is **strictly above** the value |
| `health_between` | `30-60%` | Player HP% is **inclusive** between min and max |

HP% is current health divided by max health times 100.

### Health (target)

| Condition | Value | Passes when |
|-----------|-------|-------------|
| `target_health_below` | `35%` | Target entity HP% is strictly below the value |

Requires a living target in context (usually from `attack` or `damaged`).

### Random

| Condition | Value | Passes when |
|-----------|-------|-------------|
| `chance` | `25%` | Random roll succeeds at the given percentage |

### Position and world

| Condition | Value | Passes when |
|-----------|-------|-------------|
| `biome` | `plains` | Player is in that biome (Bukkit biome key) |
| `world` | `world_nether` | Player is in that world name |
| `weather` | `CLEAR`, `RAIN`, or `THUNDER` | World weather matches |
| `time` | `DAY` or `NIGHT` | World time matches (night: 13000-22999) |
| `distance_to_target` | `5` | Target is within this many blocks (not exact distance) |

### Combat context

| Condition | Value | Passes when |
|-----------|-------|-------------|
| `has_target` | `true` | Effect context has a target entity |
| `target_is_player` | `true` | Target is a player |
| `target_type` | `ZOMBIE` | Target entity type matches (Bukkit enum name) |
| `behind_target` | `true` | Player is behind the target's facing direction |

`behind_target` uses a dot-product check on facing vectors. Value is conventionally `true`; the parser ignores the actual value.

### Player state

| Condition | Value | Passes when |
|-----------|-------|-------------|
| `is_sprinting` | `true` | Player is sprinting |
| `is_sneaking` | `true` | Player is sneaking |
| `is_blocking` | `true` | Player is blocking with a shield |
| `has_potion_effect` | `SPEED` | Player has that potion effect type |

Boolean conditions ignore the YAML value; presence of the key is enough.

### Stats and MMOCore

| Condition | Value | Passes when |
|-----------|-------|-------------|
| `has_stat` | `attack_damage` | MythicLib total for that stat is above 0 |
| `mana_above` | `50%` | MMOCore mana% is above the value |
| `mana_below` | `20%` | MMOCore mana% is below the value |
| `level_above` | `10` | MMOCore player level is above the value |
| `is_in_combat` | `true` | MMOCore combat tag is active |

`has_stat`, mana, level, and combat conditions require the relevant bridge plugins.

## Examples

**Execute window:**

```yaml
conditions:
  - has_target: true
  - target_health_below: 25%
```

**Night ambush:**

```yaml
conditions:
  - time: NIGHT
  - behind_target: true
  - target_is_player: true
```

**Low HP sustain:**

```yaml
conditions:
  - health_between: 15-45%
  - chance: 30%
```

**Ranged shot:**

```yaml
conditions:
  - has_target: true
  - distance_to_target: 18
```

## Invalid conditions

Unknown condition types log a warning. With strict validation enabled, the parent effect is skipped entirely.

## Tips

- Pair `has_target: true` with any target-based condition to avoid silent failures.
- `health_between` clamps values to 0-100 and ensures min is not above max.
- `distance_to_target` checks squared distance (within range, not at exact range).
- Conditions are checked **after** cooldown, so a failed condition does not consume cooldown.

## Related pages

- [Triggers](Triggers.md)
- [Actions](Actions.md)
- [Bridges and Integrations](Bridges-and-Integrations.md)
