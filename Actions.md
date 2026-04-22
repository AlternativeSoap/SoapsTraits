# Actions

Actions define what happens when a trigger fires and all conditions pass. They run in the order you list them.

```yaml
actions:
  - heal: 5
  - send_message: "&aVitality surge!"
  - play_sound: ENTITY_PLAYER_LEVELUP
```

---

## All Actions

### `damage_bonus`
Increases outgoing damage for the current attack. Use with `attack` or `crit` triggers.

```yaml
- damage_bonus: 25%    # +25% of the base hit
- damage_bonus: 10     # +10 flat damage
```

You can use a flat number or a percentage. Multiple `damage_bonus` actions in one effect add together.

---

### `reduce_damage`
Reduces the incoming damage the player receives. Use with the `damaged` trigger.

```yaml
- reduce_damage: 30%    # take 30% less damage
- reduce_damage: 5      # take 5 less damage (flat)
```

Damage cannot go below zero. Multiple reductions in the same effect add together.

---

### `heal`
Restores the player's health. Cannot exceed their maximum HP.

```yaml
- heal: 4
- heal: 10
```

---

### `give_mana`
Restores the player's mana through MMOCore. Requires MMOCore to be installed.

```yaml
- give_mana: 15
- give_mana: 50
```

---

### `cancel_event`
Cancels the current damage event — the player takes no damage at all. Best used on the `damaged` trigger to create a dodge mechanic.

```yaml
- cancel_event: true
```

Example dodge:

```yaml
- trigger: damaged
  conditions:
    - is_sprinting: true
    - chance: 12%
  actions:
    - cancel_event: true
    - send_message: "&7Dodged!"
    - play_sound: ENTITY_PHANTOM_FLAP
```

---

### `send_message`
Sends a chat message to the player.

```yaml
- send_message: "&aYou healed!"
- send_message: "<#FF6600>Fire activated!"
- send_message: "<gradient:red:yellow>Rage!"
```

Supports legacy color codes (`&a`, `&l`, etc.), hex colors (`<#RRGGBB>`), and MiniMessage tags.

---

### `play_sound`
Plays a sound at the player's location. Use the Bukkit `Sound` enum name (uppercase).

```yaml
- play_sound: ENTITY_PLAYER_LEVELUP
- play_sound: ENTITY_PHANTOM_FLAP
- play_sound: ENTITY_ENDER_DRAGON_GROWL
```

Common sounds: `ENTITY_PLAYER_LEVELUP`, `ITEM_ARMOR_EQUIP_IRON`, `ENTITY_ARROW_HIT_PLAYER`, `ENTITY_PHANTOM_FLAP`, `BLOCK_ANVIL_LAND`, `ENTITY_ENDER_DRAGON_GROWL`, `ENTITY_WITHER_DEATH`

---

### `set_on_fire`
Sets the **target entity** on fire for the given duration. Requires a target in context — use with `attack`, `crit`, or `kill` triggers.

```yaml
- set_on_fire: 60      # 60 ticks = 3 seconds
- set_on_fire: 3s      # same, written in seconds
```

See [Cooldowns and Durations](Cooldowns-and-Durations.md) for duration format options.

---

### `remove_potion_effect`
Removes a potion effect from the player. Use the Bukkit `PotionEffectType` name (uppercase).

```yaml
- remove_potion_effect: SLOWNESS
- remove_potion_effect: POISON
```

---

### `apply_potion_effect`
Applies a potion effect to the player.

| Property | Required | Description |
|----------|----------|-------------|
| `type` | Yes | Bukkit `PotionEffectType` name (uppercase) |
| `duration` | Yes | How long it lasts — ticks or duration string |
| `level` | No (default: 1) | Effect level: 1 = Level I, 2 = Level II, etc. |

```yaml
- apply_potion_effect:
    type: SPEED
    duration: 60
    level: 2          # Speed II for 3 seconds

- apply_potion_effect:
    type: INVISIBILITY
    duration: 5s
    level: 1
```

Common effect types: `SPEED`, `SLOWNESS`, `STRENGTH`, `WEAKNESS`, `REGENERATION`, `POISON`, `BLINDNESS`, `INVISIBILITY`, `FIRE_RESISTANCE`, `WATER_BREATHING`, `JUMP_BOOST`, `NIGHT_VISION`, `HASTE`, `ABSORPTION`, `SLOW_FALLING`

---

### `add_stat`
Temporarily adds a stat buff to the player via MythicLib. Requires MythicLib to be installed.

| Property | Required | Description |
|----------|----------|-------------|
| stat name | Yes | The stat to boost and its value |
| `duration` | No | How long the buff lasts. Omit for a permanent modifier. |

```yaml
- add_stat:
    defense: 12
    duration: 80       # 4 seconds

- add_stat:
    attack_damage: 15
    crit_chance: 10
    duration: 5s
```

You can buff multiple stats in a single `add_stat` action. See [Stats System](Stats-System.md) for all valid stat names.

---

### `spawn_particle`
Spawns particles at the player's location.

| Property | Required | Default | Description |
|----------|----------|---------|-------------|
| `type` | Yes | — | Bukkit `Particle` name (uppercase) |
| `amount` | No | `10` | How many particles |
| `spread` | No | `0.5` | Spread radius on all axes |
| `spread_x` | No | `spread` | Spread on X axis (overrides `spread`) |
| `spread_y` | No | `spread` | Spread on Y axis (overrides `spread`) |
| `spread_z` | No | `spread` | Spread on Z axis (overrides `spread`) |
| `speed` | No | `0` | Particle speed |

```yaml
- spawn_particle:
    type: FLAME
    amount: 15
    spread: 0.3
    speed: 0.05

- spawn_particle:
    type: HEART
    amount: 5
```

Common particle types: `FLAME`, `CRIT`, `SMOKE`, `ENCHANT`, `HEART`, `DAMAGE_INDICATOR`, `END_ROD`, `SWEEP_ATTACK`, `TOTEM_OF_UNDYING`, `SOUL_FIRE_FLAME`

---

> **Next:** [Stats System →](Stats-System.md)
