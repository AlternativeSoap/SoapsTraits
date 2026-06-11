# Examples

These examples match the bundled `traits.yml` in SoapsTraits 1.0.0. Copy sections into your own traits or study them for patterns.

## Human: low HP sustain

Emergency heal with cleanse when health is mid-low:

```yaml
human:
  class: HUMAN
  stats:
    max_health: 3
    health_regen: 0.6
    mana_regen: 0.6
  effects:
    - trigger: damaged
      cooldown: 160
      conditions:
        - health_between: 15-45%
        - chance: 30%
      actions:
        - heal: 2
        - clear_negative_effects: true
        - send_message: "&eYou steady yourself."

    - trigger: tick
      cooldown: 120
      conditions:
        - health_below: 70%
      actions:
        - heal: 1
        - feed:
            food: 1
            saturation: 0.5
```

**Pattern:** `tick` + cooldown for passive regen; `damaged` + `chance` for a proc.

## Warrior: execute and shield brace

```yaml
warrior:
  class: WARRIOR
  stats:
    max_health: 8
    defense: 7
    attack_damage: 3
  effects:
    - trigger: attack
      cooldown: 80
      conditions:
        - has_target: true
        - target_health_below: 35%
      actions:
        - damage_bonus: 20%
        - knockback_target: 0.8
        - play_sound: ENTITY_PLAYER_ATTACK_STRONG

    - trigger: damaged
      cooldown: 100
      conditions:
        - is_blocking: true
      actions:
        - reduce_damage: 28%
        - clear_negative_effects: true
        - spawn_particle:
            type: CRIT
            amount: 8
            spread: 0.35
            speed: 0.02
```

**Pattern:** `has_target` before target conditions; `is_blocking` for shield gameplay.

## Rogue: backstab and dodge

```yaml
rogue:
  class: ROGUE
  effects:
    - trigger: attack
      cooldown: 60
      conditions:
        - has_target: true
        - behind_target: true
        - target_is_player: true
      actions:
        - damage_bonus: 28%
        - spawn_particle:
            type: SMOKE
            amount: 10
            spread: 0.25
            speed: 0.01

    - trigger: damaged
      cooldown: 140
      conditions:
        - is_sprinting: true
        - chance: 14%
      actions:
        - cancel_event: true
        - teleport_forward: 3
        - play_sound: ENTITY_PHANTOM_FLAP
```

**Pattern:** `cancel_event` on `damaged` for dodge; positional `behind_target` check.

## Mage: mana refund and blink

```yaml
mage:
  class: MAGE
  effects:
    - trigger: skill_cast
      cooldown: 100
      conditions:
        - mana_below: 45%
        - chance: 35%
      actions:
        - give_mana: 18
        - clear_negative_effects: true

    - trigger: damaged
      cooldown: 200
      conditions:
        - health_between: 20-60%
        - chance: 22%
      actions:
        - teleport_forward: 4
        - send_message: "&bArcane blink!"
```

**Pattern:** `skill_cast` for MMOCore synergy; defensive teleport on damage.

## Berserker: rage scaling

```yaml
berserker:
  class: BERSERKER
  effects:
    - trigger: attack
      cooldown: 60
      conditions:
        - health_below: 45%
      actions:
        - damage_bonus: 30%

    - trigger: damaged
      cooldown: 180
      conditions:
        - health_between: 10-35%
      actions:
        - add_stat:
            defense: 14
            duration: 80
        - send_message: "&4Rage hardens your resolve!"

    - trigger: kill
      cooldown: 100
      conditions:
        - health_below: 60%
      actions:
        - heal: 4
        - clear_negative_effects: true
```

**Pattern:** health gates for risk/reward; `add_stat` for short defensive window; `kill` sustain.

## Showcase: night shadowstep (no class)

```yaml
shadowstep_showcase:
  stats:
    speed: 0.02
  effects:
    - trigger: crouch
      cooldown: 160
      conditions:
        - time: NIGHT
        - health_between: 30-90%
      actions:
        - teleport_forward: 5
        - apply_potion_effect:
            type: INVISIBILITY
            duration: 60
            level: 1
        - play_sound: ENTITY_ENDERMAN_TELEPORT
```

**Pattern:** `crouch` trigger for active ability; no `class` so assign with `/sts set`.

## Minimal custom trait

```yaml
traits:
  scout:
    stats:
      speed: 0.03
    effects:
      - trigger: attack
        cooldown: 100
        conditions:
          - distance_to_target: 12
        actions:
          - damage_bonus: 12%
```

Assign manually: `/sts set PlayerName scout`

## Testing a trait

```
/sts test scout PlayerName
```

Fires assignment immediately for live combat testing.

## Related pages

- [Traits Configuration](Traits-Configuration.md)
- [Triggers](Triggers.md)
- [Conditions](Conditions.md)
- [Actions](Actions.md)
