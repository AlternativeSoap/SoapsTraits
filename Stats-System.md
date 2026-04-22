# Stats System

SoapsTraits uses MythicLib to apply stat bonuses to players. Any trait can define base stats that are active as long as the player has that trait, and effects can grant temporary stat buffs through the `add_stat` action.

---

## Supported Stats

| Name | What it does |
|------|-------------|
| `attack_damage` | Flat bonus to melee and projectile damage |
| `crit_chance` | Percentage chance to land a critical hit |
| `crit_damage` | Bonus damage multiplier on critical hits |
| `defense` | Reduces incoming damage |
| `max_health` | Bonus maximum health |
| `speed` | Bonus movement speed |
| `health_regen` | HP regenerated per interval |
| `mana` | Bonus maximum mana pool |
| `mana_regen` | Mana regenerated per interval |
| `cooldown_reduction` | Reduces skill cooldown durations |

All values are additive bonuses on top of the player's existing stats. The exact numerical impact depends on your MythicLib configuration — test in-game to calibrate values.

---

## Base Stats (Permanent)

Stats defined in a trait's `stats:` block stay active for as long as the player has that trait:

```yaml
warrior:
  stats:
    max_health: 8
    defense: 6
    attack_damage: 3
```

- Applied when the trait is assigned (on join, class change, or `/sts set`)
- Removed automatically when the trait is changed or the player disconnects
- Re-applied on reconnect and after respawn

---

## Temporary Stat Buffs

The `add_stat` action applies a stat bonus for a limited duration. Once the timer expires, the bonus is removed automatically.

```yaml
- trigger: damaged
  cooldown: 100
  conditions:
    - chance: 20%
  actions:
    - add_stat:
        defense: 12
        duration: 80        # 80 ticks = 4 seconds
```

See [Actions](Actions.md) for full `add_stat` usage.

---

## Using Custom Stats

If your MythicLib setup includes custom stats beyond the defaults above, you can use them by name:

```yaml
stats:
  my_custom_stat: 10
```

Unrecognized stat names are passed directly to MythicLib. Make sure the name matches exactly what MythicLib expects (it will be uppercased automatically).

---

## `has_stat` Condition

The `has_stat` condition passes if the player's current value for a stat is greater than zero:

```yaml
conditions:
  - has_stat: attack_damage    # passes if the player has any attack damage bonus
```

Useful for enabling effects only for classes that have specific stat bonuses.

---

> **Next:** [Cooldowns & Durations →](Cooldowns-and-Durations.md)
