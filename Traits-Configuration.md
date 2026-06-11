# Traits Configuration

All traits live in `plugins/SoapsTraits/traits.yml` under the top-level `traits:` key.

## File structure

```yaml
traits:
  trait_id:
    class: MMOCore_CLASS_ID    # optional
    stats:                     # optional
      attack_damage: 3
      defense: 5
    effects:                   # optional list
      - trigger: attack
        cooldown: 80
        conditions:
          - chance: 25%
        actions:
          - heal: 2
```

## Trait ID rules

Trait names are normalized to lowercase. Valid IDs match:

- 3 to 32 characters
- Letters, numbers, underscores (`_`), hyphens (`-`)

Examples: `warrior`, `shadowstep_showcase`, `my-trait`

Invalid names are rejected when saving from the GUI.

## Class binding (optional)

```yaml
warrior:
  class: WARRIOR
```

When MMOCore is installed, players who pick the `WARRIOR` class receive this trait automatically on join or class change. The `class` value must match the MMOCore profession ID exactly (case-insensitive match at runtime).

Traits without `class` are only assigned manually with `/sts set` or kept as showcase/default options.

## Stats block (optional)

```yaml
stats:
  max_health: 8
  attack_damage: 3
  crit_chance: 11
```

Stat keys use lowercase with underscores. See [Stats System](Stats-System.md) for the full list and MythicLib mapping.

Stat keys must match `^[a-z0-9_.-]{2,40}$` when created through the GUI.

## Effects list

Each effect is one list entry with these fields:

| Field | Required | Description |
|-------|----------|-------------|
| `trigger` | Yes | When to evaluate this effect. See [Triggers](Triggers.md). |
| `conditions` | No | List of checks. All must pass. See [Conditions](Conditions.md). |
| `actions` | Yes | List of results. At least one valid action required. See [Actions](Actions.md). |
| `cooldown` | No | Ticks before this effect can fire again. Default: `0` (no cooldown). |

### Effect example

```yaml
effects:
  - trigger: damaged
    cooldown: 160
    conditions:
      - health_below: 40%
      - chance: 30%
    actions:
      - heal: 4
      - send_message: "&aYou recover!"
```

## Condition and action YAML format

Each condition and action is a single-key map in a list:

```yaml
conditions:
  - health_below: 50%
  - has_target: true

actions:
  - damage_bonus: 20%
  - play_sound: ENTITY_PLAYER_LEVELUP
```

Complex actions use nested maps:

```yaml
actions:
  - apply_potion_effect:
      type: SPEED
      duration: 60
      level: 2
  - add_stat:
      defense: 14
      duration: 80
```

## Strict validation

When `settings.strict-config-validation` is `true` in `config.yml` (default):

- One invalid condition or action in an effect causes the **whole effect** to be dropped at load.
- The server logs a warning naming the bad entry.

When `false`, invalid entries are skipped but valid ones in the same effect still load.

## Default bundled traits

The shipped `traits.yml` includes:

| Trait | Class binding |
|-------|---------------|
| `human` | HUMAN |
| `warrior` | WARRIOR |
| `mage` | MAGE |
| `rogue` | ROGUE |
| `paladin` | PALADIN |
| `archer` | ARCHER |
| `berserker` | BERSERKER |
| `shadowstep_showcase` | none |
| `executioner_showcase` | none |
| `guardian_showcase` | none |

Showcase traits demonstrate advanced combinations without class binding.

## Balancing notes (from defaults)

The bundled file includes these guidelines:

- `chance`: usually 8% to 35%
- `damage_bonus`: usually 8% to 35%
- `reduce_damage`: usually below 45%
- `teleport_forward`: usually 2 to 6 blocks
- `knockback_target`: usually 0.4 to 1.4
- `tick` trigger: always pair with a cooldown

## Saving changes

- **Manual:** Edit `traits.yml`, then `/sts reload`.
- **GUI:** Changes save directly to `traits.yml` with automatic backup (`traits.yml.bak`).
- **Import:** `/sts import <file.yml>` replaces `traits.yml` entirely. See [Import Export](Import-Export.md).

## Related pages

- [Triggers](Triggers.md)
- [Conditions](Conditions.md)
- [Actions](Actions.md)
- [Cooldowns and Durations](Cooldowns-and-Durations.md)
