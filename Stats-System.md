# Stats System

Trait stats are passive bonuses applied while a player has that trait. They are defined under the `stats:` block in `traits.yml` and applied through **MythicLib** stat modifiers.

## Supported stat keys

| YAML key | MythicLib stat |
|----------|----------------|
| `attack_damage` | ATTACK_DAMAGE |
| `crit_chance` | CRITICAL_STRIKE_CHANCE |
| `crit_damage` | CRITICAL_STRIKE_POWER |
| `defense` | DEFENSE |
| `max_health` | MAX_HEALTH |
| `speed` | MOVEMENT_SPEED |
| `health_regen` | HEALTH_REGENERATION |
| `mana` | MAX_MANA |
| `mana_regen` | MANA_REGENERATION |
| `cooldown_reduction` | COOLDOWN_REDUCTION |

Custom stat names are passed to MythicLib in uppercase if they are not in the alias table.

## Example

```yaml
warrior:
  stats:
    max_health: 8
    defense: 7
    attack_damage: 3
```

Values are additive modifiers registered with keys like `soapstraits-base-warrior-attack_damage`.

## When stats apply

- On player join (short delay for MMOCore/MythicLib load)
- On trait assignment (`/sts set`, class change, `ensureTrait`)
- On respawn
- After `/sts reload` (all online players refreshed)

Stats are removed on quit and before trait swaps to avoid stacking.

## Temporary stat buffs (`add_stat` action)

The `add_stat` action applies **temporary** modifiers separate from base trait stats:

```yaml
actions:
  - add_stat:
      defense: 14
      duration: 80
```

- Duration is in ticks (`80` = 4 seconds).
- Modifier keys use the pattern `soapstraits-<trait>-<stat>`.
- When duration ends, the modifier is removed automatically.
- Requires MythicLib.

## `has_stat` condition

```yaml
conditions:
  - has_stat: attack_damage
```

Passes when the player's MythicLib total for that stat is greater than zero. Useful for synergy checks, not for exact thresholds.

## Viewing stats

Players:

```
/sts info
```

Shows base stat modifiers on the active trait.

Admins viewing runtime data:

```
/sts stats <player>
```

Shows per-player runtime stat map from `playerdata.yml` (used for tracking, separate from MythicLib).

## MythicLib not installed

Without MythicLib:

- Base trait stats in `traits.yml` are loaded but **not applied**.
- `add_stat` actions do nothing.
- `has_stat` conditions always fail.

Install MythicLib for any stat-related gameplay.

## Balancing tips

- Small values go a long way with MythicLib scaling. Start low and test with `/sts test`.
- `speed` values are typically decimals (example: `0.035`).
- `max_health` and `mana` stack with gear and other plugins through MythicLib.

## Related pages

- [Actions](Actions.md) - `add_stat` details
- [Bridges and Integrations](Bridges-and-Integrations.md) - MythicLib bridge
- [Traits Configuration](Traits-Configuration.md)
