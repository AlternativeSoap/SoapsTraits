# Triggers

A trigger defines **when** an effect is considered. Each effect in `traits.yml` has exactly one trigger.

Trigger names are case-insensitive in YAML. They are stored internally in uppercase.

## All triggers (1.0.0)

| Trigger | Fires when |
|---------|------------|
| `attack` | The player deals damage to an entity (melee, projectile, or direct) |
| `damaged` | The player takes damage from any source |
| `kill` | The player kills an entity |
| `crit` | The player lands a critical hit (same hit as `attack`, evaluated after) |
| `skill_cast` | The player casts an MMOCore skill |
| `tick` | Repeating timer for all online players (see interval below) |
| `death` | The player dies |
| `respawn` | The player respawns (slight delay after respawn) |
| `crouch` | The player **starts** sneaking (not when they stop) |

## YAML usage

```yaml
effects:
  - trigger: attack
    conditions:
      - has_target: true
    actions:
      - damage_bonus: 15%
```

## Combat triggers in detail

### attack

- Fires for the attacking player before damage is finalized.
- Sets the **target** to the entity being hit.
- `damage_bonus` and `cancel_event` apply to this outgoing hit.
- Projectile attacks resolve the shooter as the player.

### damaged

- Fires for the victim after the attacker's `attack` processing (unless the attack was cancelled).
- Sets the **target** to the damager (may be null for environmental damage).
- `reduce_damage` and `cancel_event` apply to this incoming hit (dodge mechanic).

### crit

- Fires on the same damage event as `attack` when the hit qualifies as a critical strike.
- Uses vanilla crit detection (falling, not on ground, etc.).
- Useful for finisher effects separate from normal attacks.

### kill

- Fires when `entity.getKiller()` returns the player.
- Target is the slain entity.

## MMOCore trigger

### skill_cast

- Registered through MMOCore's skill cast event (requires MMOCore on the server).
- Use for mana refunds, buffs on cast, or cooldown synergy.

## Passive trigger

### tick

- Runs on a server-wide timer for every online player with traits enabled.
- Interval is set in `config.yml`:

```yaml
settings:
  tick-interval: 20   # 20 ticks = 1 second
```

- Always add a `cooldown` on tick effects to avoid running every second.
- No combat target is set unless the player is currently fighting.

## Life cycle triggers

### death

- Fires on player death.
- Target is set to the killer if one exists.

### respawn

- Fires shortly after respawn.
- Trait base stats are re-applied on respawn before this trigger runs.

### crouch

- Fires only when sneak is **pressed**, not released.
- Useful for mobility abilities like shadow step.

## Effect processing order

For each trigger on a trait:

1. Cooldown checked first (skip if on cooldown).
2. All conditions evaluated (all must pass).
3. Cooldown marked as used.
4. Actions run in list order.

Multiple effects on the same trait can share a trigger. Each has its own cooldown and condition set.

## Toggle respect

If a player runs `/sts toggle` and disables their trait, **no triggers fire**. Base stats from the trait still apply until the trait is changed.

## Debug

Use `/sts debug <player>` to see trigger lines in chat:

```
[Trigger] ATTACK fired
```

Enable `debug-mode: true` in `config.yml` for console logging as well.

## Related pages

- [Conditions](Conditions.md)
- [Actions](Actions.md)
- [Cooldowns and Durations](Cooldowns-and-Durations.md)
