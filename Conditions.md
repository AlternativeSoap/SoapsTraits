# Conditions

Conditions are checks that all must pass before an effect's actions run. If any one fails, the effect is skipped entirely for that event — and the cooldown is not consumed. Conditions are evaluated in order.

```yaml
effects:
  - trigger: attack
    conditions:
      - health_below: 30%
      - chance: 25%
      - is_sprinting: true
    actions:
      - damage_bonus: 40%
```

---

## All Conditions

### `health_below`
Passes if the player's current health is below the given percentage.

```yaml
- health_below: 30%    # fires when HP < 30%
- health_below: 50%
```

---

### `health_above`
Passes if the player's current health is above the given percentage.

```yaml
- health_above: 90%    # fires when HP > 90%
- health_above: 70%
```

---

### `chance`
Passes randomly with the given probability. A fresh roll happens every time the effect is evaluated.

```yaml
- chance: 25%     # 25% chance to pass
- chance: 10%
- chance: 100%    # always passes
```

---

### `biome`
Passes if the player is currently in the specified biome. Use the Bukkit biome name (case-insensitive).

```yaml
- biome: plains
- biome: dark_forest
- biome: nether_wastes
```

Common biome names: `plains`, `forest`, `desert`, `jungle`, `swamp`, `dark_forest`, `nether_wastes`, `soul_sand_valley`, `crimson_forest`, `the_end`, `end_highlands`

---

### `time`
Passes based on the current in-game time. Only `DAY` and `NIGHT` are accepted.

```yaml
- time: NIGHT     # ticks 13000–22999
- time: DAY       # any other time
```

---

### `distance_to_target`
Passes if the player is **within** the specified number of blocks from their target. Requires a target to be in context (use with `attack`, `damaged`, `kill`, `crit`).

```yaml
- distance_to_target: 10    # passes when within 10 blocks
- distance_to_target: 5
```

---

### `behind_target`
Passes if the player is positioned behind their target — in roughly the rear 90° arc. Requires a target in context.

```yaml
- behind_target: true
```

Classic backstab check. Great for Rogue-type classes.

---

### `has_stat`
Passes if the player's current value for the given stat is greater than zero. Requires MythicLib.

```yaml
- has_stat: attack_damage
- has_stat: crit_chance
```

Uses the same stat names as the `stats:` block. See [Stats System](Stats-System.md).

---

### `is_sprinting`
Passes if the player is currently sprinting.

```yaml
- is_sprinting: true
```

---

### `is_sneaking`
Passes if the player is currently holding sneak.

```yaml
- is_sneaking: true
```

---

### `is_blocking`
Passes if the player is actively blocking with a shield.

```yaml
- is_blocking: true
```

---

### `is_in_combat`
Passes if the player has the MMOCore combat tag active. Requires MMOCore — always returns false if MMOCore isn't available.

```yaml
- is_in_combat: true
```

---

### `has_potion_effect`
Passes if the player currently has the specified potion effect. Use the Bukkit `PotionEffectType` name (uppercase).

```yaml
- has_potion_effect: SPEED
- has_potion_effect: INVISIBILITY
- has_potion_effect: REGENERATION
```

Common effect names: `SPEED`, `SLOWNESS`, `STRENGTH`, `WEAKNESS`, `REGENERATION`, `POISON`, `BLINDNESS`, `INVISIBILITY`, `FIRE_RESISTANCE`, `WATER_BREATHING`

---

### `target_type`
Passes if the target entity matches the given entity type. Requires a target in context. Use the Bukkit `EntityType` name (uppercase).

```yaml
- target_type: ZOMBIE
- target_type: SKELETON
- target_type: PLAYER
```

Common entity types: `ZOMBIE`, `SKELETON`, `CREEPER`, `ENDERMAN`, `PLAYER`, `VILLAGER`, `WITHER`, `ENDER_DRAGON`

---

### `world`
Passes if the player is in the specified world. The world name must match exactly (case-sensitive).

```yaml
- world: world
- world: world_nether
- world: world_the_end
- world: my_custom_world
```

---

### `mana_above`
Passes if the player's current mana is above the given percentage. Requires both MMOCore (current mana) and MythicLib (max mana) — always returns false if either is unavailable.

```yaml
- mana_above: 60%
- mana_above: 80%
```

---

### `mana_below`
Passes if the player's current mana is below the given percentage. Same requirements as `mana_above`.

```yaml
- mana_below: 20%
- mana_below: 50%
```

---

### `level_above`
Passes if the player's MMOCore level is above the specified value. Requires MMOCore.

```yaml
- level_above: 10     # passes at level 11+
- level_above: 25
```

---

## Combining Conditions

All conditions must pass. Combine them to create nuanced, situational effects:

```yaml
# Backstab: only from behind, only while sneaking, 50% chance
- trigger: attack
  conditions:
    - is_sneaking: true
    - behind_target: true
    - chance: 50%
  actions:
    - damage_bonus: 60%
    - send_message: "&7Critical backstab!"
```

```yaml
# Tick-based heal: only at night, only below 40% HP
- trigger: tick
  cooldown: 100
  conditions:
    - time: NIGHT
    - health_below: 40%
  actions:
    - heal: 3
```

---

> **Next:** [Actions →](Actions.md)
