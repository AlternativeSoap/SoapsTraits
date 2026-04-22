# Triggers

A trigger defines **when** an effect can activate. Every effect must have exactly one trigger. When the matching event occurs for a player, the effect's conditions are checked and — if they all pass — the actions run.

---

## All Triggers

| Trigger | When it fires |
|---------|---------------|
| `attack` | The player deals damage to an entity (melee or projectile) |
| `damaged` | The player takes damage from an entity |
| `kill` | The player kills an entity |
| `crit` | The player lands a critical hit |
| `skill_cast` | The player casts an MMOCore skill |
| `tick` | Fires periodically on a configurable interval |
| `death` | The player dies |
| `respawn` | The player respawns after dying |
| `crouch` | The player starts sneaking |

---

## Trigger Details

### `attack`
Fires when the player deals damage to any entity — melee or projectile (arrows, tridents, etc.). The target entity is available to conditions and actions.

```yaml
- trigger: attack
  actions:
    - damage_bonus: 10%
```

Actions like `damage_bonus` will modify the event's damage for this hit.

---

### `damaged`
Fires when the player takes damage from an entity. The attacker is available as the target.

```yaml
- trigger: damaged
  conditions:
    - health_below: 30%
  actions:
    - reduce_damage: 20%
```

Actions like `reduce_damage` and `cancel_event` work on this trigger.

---

### `kill`
Fires when the player kills any living entity. The killed entity is available as the target.

```yaml
- trigger: kill
  actions:
    - heal: 3
    - send_message: "&aVictory!"
```

---

### `crit`
Fires when the player lands a vanilla Minecraft critical hit (jumping attack). Both `attack` and `crit` fire on the same hit — `attack` first, then `crit`. Damage modifications from both stack.

```yaml
- trigger: crit
  actions:
    - damage_bonus: 50%
    - spawn_particle:
        type: CRIT
        amount: 15
```

---

### `skill_cast`
Fires when the player casts a skill through MythicLib/MMOCore. No target is available in context.

```yaml
- trigger: skill_cast
  cooldown: 60
  actions:
    - give_mana: 15
    - send_message: "&bArcane surge!"
```

> This trigger requires a compatible version of MythicLib. If the required skill event class isn't found at startup, this trigger simply won't fire — everything else works normally.

---

### `tick`
Fires periodically for all online players with a trait. The interval is set in `config.yml`:

```yaml
# config.yml
settings:
  tick-interval: 20    # 20 ticks = 1 second
```

Use a cooldown on tick effects to avoid them firing every single tick:

```yaml
- trigger: tick
  cooldown: 200         # Fire at most once every 10 seconds
  conditions:
    - health_below: 50%
  actions:
    - heal: 1
```

No target is available in context.

---

### `death`
Fires when the player dies. If the player was killed by another player, that killer is available as the target.

```yaml
- trigger: death
  actions:
    - send_message: "&cYou have fallen..."
```

---

### `respawn`
Fires when the player respawns after death. Trait stats are re-applied before this trigger fires. No target is available.

```yaml
- trigger: respawn
  actions:
    - apply_potion_effect:
        type: REGENERATION
        duration: 100
        level: 2
    - send_message: "&aYou rise again!"
```

---

### `crouch`
Fires when the player **starts** sneaking. Does not fire when they stop sneaking.

```yaml
- trigger: crouch
  cooldown: 400
  actions:
    - apply_potion_effect:
        type: INVISIBILITY
        duration: 60
        level: 1
    - send_message: "&7Shadow cloak..."
```

---

## Multiple Effects with the Same Trigger

You can have as many effects per trigger as you want. They are processed in the order they appear in `traits.yml`.

For combat events, the order is:
1. Attacker's `attack` trigger runs, damage bonus applied
2. Attacker's `crit` trigger runs (if a crit), further damage bonus applied
3. Final damage written to the event
4. Victim's `damaged` trigger runs (unless the event was cancelled)
5. Damage reductions applied

---

> **Next:** [Conditions →](Conditions.md)
