# Traits Configuration

Traits are defined in `plugins/SoapsTraits/traits.yml`. Each trait has a name, optional stat bonuses, and a list of effects that trigger during gameplay.

---

## Basic Structure

```yaml
traits:
  trait_name:
    class: CLASS_NAME        # Optional: auto-assign when player picks this MMOCore class
    stats:                   # Optional: permanent stat bonuses
      stat_name: value
    effects:                 # Optional: list of triggered effects
      - trigger: trigger_type
        cooldown: value      # Optional
        conditions:          # Optional
          - condition_type: value
        actions:             # Required
          - action_type: value
```

---

## Trait Properties

### `class`
Links the trait to an MMOCore class. When a player selects that class, this trait is assigned automatically — no commands needed.

```yaml
warrior:
  class: WARRIOR
```

The class name must match the MMOCore class ID (case-insensitive). If you leave this out, the trait can only be assigned manually with `/sts set <player> <trait>`.

Only one trait can be bound to each class.

---

### `stats`
Permanent stat bonuses that are active as long as the player has this trait. They apply on login, class change, or when you use `/sts set`, and are removed when the trait changes or the player disconnects.

```yaml
stats:
  max_health: 8
  defense: 6
  attack_damage: 3
```

See [Stats System](Stats-System.md) for all available stat names.

---

### `effects`
A list of triggered effects. Each effect fires when its trigger matches and all its conditions pass.

```yaml
effects:
  - trigger: attack
    cooldown: 100
    conditions:
      - health_below: 30%
    actions:
      - damage_bonus: 25%
      - send_message: "&cRage!"
```

---

## Effect Properties

### `trigger` (Required)
The event that can activate this effect. See [Triggers](Triggers.md) for all options.

### `cooldown` (Optional)
How long before this effect can fire again, per player. Tracked independently — two players won't share each other's cooldowns. Defaults to `0` (no cooldown).

```yaml
cooldown: 100    # 100 ticks = 5 seconds
cooldown: 5s     # Same, written in seconds
```

See [Cooldowns & Durations](Cooldowns-and-Durations.md) for the full syntax.

### `conditions` (Optional)
A list of checks that all must pass before the actions run. If any condition fails, the whole effect is skipped for that event — and the cooldown is not consumed.

```yaml
conditions:
  - health_below: 30%
  - chance: 25%
  - is_sprinting: true
```

See [Conditions](Conditions.md) for all options.

### `actions` (Required)
What happens when the trigger fires and all conditions pass. Actions execute in the order they're listed.

```yaml
actions:
  - damage_bonus: 25%
  - heal: 4
  - send_message: "&aHealed!"
```

See [Actions](Actions.md) for all options.

---

## Multiple Effects per Trait

A trait can have as many effects as you want. Each one has its own trigger, conditions, and cooldown.

```yaml
warrior:
  class: WARRIOR
  stats:
    max_health: 8
    defense: 6

  effects:
    # Chance to gain a temporary defense buff when hit
    - trigger: damaged
      cooldown: 100
      conditions:
        - chance: 20%
      actions:
        - add_stat:
            defense: 12
            duration: 80

    # Heal on kill
    - trigger: kill
      conditions:
        - chance: 30%
      actions:
        - heal: 3
        - send_message: "&6For glory!"

    # Slow regen when low HP
    - trigger: tick
      conditions:
        - health_below: 50%
      actions:
        - heal: 1
```

---

## Default Trait

The `default-trait` setting in `config.yml` is the fallback for players who:
- Join for the first time
- Don't have a class-bound trait when changing classes
- Have their trait removed via `/sts remove`

```yaml
# config.yml
default-trait: human
```

Players always have a trait — removing one just assigns the default.

---

## Class Auto-Assignment

When a player changes their class in MMOCore, SoapsTraits checks if any trait has a matching `class:` binding. If found, that trait is assigned automatically. If not, the player keeps their current trait or falls back to the default.

---

## Naming

- Trait names are case-insensitive and stored in lowercase
- Multi-word names: use underscores (`dark_knight`) or hyphens (`shadow-mage`)
- Class names in `class:` should match the MMOCore class ID exactly (case-insensitive match)

---

## Validation

If a trait fails to load due to invalid YAML or an unknown trigger/condition/action name, SoapsTraits will log a warning and skip that trait. Other traits will continue to load normally.

Enable `debug-mode: true` in `config.yml` to see detailed loading information.

---

> **Next:** [Triggers →](Triggers.md)
