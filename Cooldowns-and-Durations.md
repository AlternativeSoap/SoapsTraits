# Cooldowns and Durations

SoapsTraits uses **ticks** for all time values unless noted otherwise.

```
20 ticks = 1 second
```

## Effect cooldowns

Each effect can set a cooldown that prevents it from firing again until the time expires:

```yaml
- trigger: attack
  cooldown: 80        # 4 seconds
  conditions:
    - chance: 25%
  actions:
    - damage_bonus: 20%
```

### Parsing rules

- Plain number: ticks (`80` = 80 ticks).
- Optional `t` suffix: `80t` also means 80 ticks.
- Default: `0` (no cooldown).
- Negative values are clamped to `0`.
- Maximum is capped by `settings.max-cooldown-ticks` in `config.yml` (default: `72000` = 1 hour).

### Behavior

- Cooldown is checked **before** conditions. A failed condition does not start cooldown.
- Cooldown starts when conditions pass and actions are about to run.
- Cooldown is **per player, per effect** (stored in memory, cleared on quit).
- Each effect on a trait has its own cooldown timer.

### Tick trigger reminder

Passive `tick` effects run every `tick-interval` ticks (default 20). Always set a cooldown high enough to match your intended rate:

```yaml
settings:
  tick-interval: 20   # check every 1 second

# In traits.yml:
- trigger: tick
  cooldown: 120       # fire at most every 6 seconds
```

## Durations (temporary effects)

These action fields use tick durations:

| Action | Field | Notes |
|--------|-------|-------|
| `add_stat` | `duration` | `-1` or omitted = permanent until removed |
| `apply_potion_effect` | `duration` | Default 100 ticks if omitted |
| `set_on_fire` | value | Fire ticks on target. Default 60 if invalid |

Duration parsing accepts the same `t` suffix (`100t`).

## Admin cooldown command

```
/sts cooldown <player> <trait> <ticks>
```

Permission: `soapstraits.admin.cooldown`

This writes a cooldown timestamp into the player's entry in `playerdata.yml`. It is stored for persistence but **does not** replace per-effect cooldowns in `traits.yml`. Use effect `cooldown` fields to control ability timing in gameplay.

## Player toggle

`/sts toggle` disables all effect triggers for a player. Cooldown timers are not the same as toggle; toggle is an on/off switch for the whole trait's effects.

## Reset cooldowns

- `/sts reset <player>` clears player trait data including stored cooldowns and runtime stats.
- Quitting clears in-memory effect cooldowns for that player.
- Changing traits calls `clearRuntimeState` for effect cooldown maps.

## Config safety cap

```yaml
settings:
  max-cooldown-ticks: 72000
```

Values above this in `traits.yml` are clamped at load with a console warning.

## Related pages

- [Traits Configuration](Traits-Configuration.md)
- [Triggers](Triggers.md) - especially `tick`
- [Commands and Permissions](Commands-and-Permissions.md)
