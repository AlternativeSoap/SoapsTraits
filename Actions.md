# Actions

Actions run when a trigger fires and all conditions pass. They execute **in list order** within one effect.

## YAML format

Simple actions use a single key and value:

```yaml
actions:
  - heal: 4
  - damage_bonus: 25%
  - send_message: "&aHit!"
```

Map actions use nested keys:

```yaml
actions:
  - apply_potion_effect:
      type: SPEED
      duration: 60
      level: 2
```

## All actions (1.0.0)

### Combat

| Action | Value | Effect |
|--------|-------|--------|
| `damage_bonus` | `25%` or `5` | Adds percent or flat bonus to outgoing damage on `attack`/`crit` |
| `reduce_damage` | `30%` or `5` | Reduces incoming damage on `damaged` |
| `cancel_event` | `true` | Cancels the combat event (dodge). Value ignored. |
| `knockback_target` | `1.2` | Pushes the current target away from the player |
| `set_on_fire` | `60` | Ignites the target for this many ticks (default 60 if invalid) |

Percent values use a `%` suffix. Plain numbers are flat amounts.

### Recovery

| Action | Value | Effect |
|--------|-------|--------|
| `heal` | `4` | Heals the player by this many HP |
| `feed` | `6` or map | Restores hunger. Map: `food` and `saturation` keys |
| `give_mana` | `10` | Restores MMOCore mana |
| `clear_negative_effects` | `true` | Removes common debuffs from the player |
| `remove_potion_effect` | `SLOWNESS` | Removes one potion effect type |

**Feed map example:**

```yaml
- feed:
    food: 2
    saturation: 1
```

If `feed` is a plain number, saturation defaults to `0`.

### Stats and buffs

| Action | Value | Effect |
|--------|-------|--------|
| `add_stat` | map | Temporary MythicLib stat buff |

```yaml
- add_stat:
    defense: 14
    duration: 80
```

- All keys except `duration` are stat names with numeric values.
- `duration` is in ticks. Omit or use `-1` for permanent until removed manually.
- Requires MythicLib.

### Potion effects

| Action | Value | Effect |
|--------|-------|--------|
| `apply_potion_effect` | map | Applies a potion effect to the player |

```yaml
- apply_potion_effect:
    type: SPEED
    duration: 60
    level: 1
```

- `type`: Bukkit `PotionEffectType` name.
- `duration`: ticks.
- `level`: potion level (1 = Speed I, 2 = Speed II). `amplifier` is accepted as an alias for `level`.
- Defaults: `SPEED`, 100 ticks, level 1.

### Movement

| Action | Value | Effect |
|--------|-------|--------|
| `teleport_forward` | `4` | Teleports the player forward by this many blocks (eye direction) |

### Feedback

| Action | Value | Effect |
|--------|-------|--------|
| `send_message` | `"&aHello!"` | Sends a chat message to the player (`&` color codes supported) |
| `play_sound` | `ENTITY_PLAYER_LEVELUP` | Plays a sound at the player's location |
| `spawn_particle` | map or string | Spawns particles at the player |

**Particle map:**

```yaml
- spawn_particle:
    type: FLAME
    amount: 15
    spread: 0.3
    speed: 0.05
```

| Key | Default | Description |
|-----|---------|-------------|
| `type` | `FLAME` | Bukkit `Particle` name |
| `amount` / `count` | `10` | Particle count |
| `spread` | `0.5` | Shorthand for all axes |
| `spread_x`, `spread_y`, `spread_z` | from `spread` | Per-axis spread |
| `speed` | `0` | Particle velocity |

A string value uses that particle type with default count and spread.

## Combat action notes

- `damage_bonus` stacks additively within one hit (flat plus percent).
- `reduce_damage` applies on the `damaged` trigger before final damage.
- `cancel_event` on `damaged` prevents the hit entirely (rogue dodge pattern).
- `knockback_target` needs a valid target in context.

## Invalid actions

Unknown action types log a warning. With `strict-config-validation: true`, the whole effect is skipped.

An effect with zero valid actions is always skipped.

## Examples

**Warrior execute:**

```yaml
actions:
  - damage_bonus: 20%
  - knockback_target: 0.8
  - play_sound: ENTITY_PLAYER_ATTACK_STRONG
```

**Mage blink:**

```yaml
actions:
  - teleport_forward: 4
  - send_message: "&bArcane blink!"
  - play_sound: ENTITY_ENDERMAN_TELEPORT
```

**Paladin cleanse:**

```yaml
actions:
  - heal: 4
  - clear_negative_effects: true
  - play_sound: BLOCK_AMETHYST_BLOCK_CHIME
```

## Related pages

- [Triggers](Triggers.md)
- [Conditions](Conditions.md)
- [Stats System](Stats-System.md)
- [Cooldowns and Durations](Cooldowns-and-Durations.md)
