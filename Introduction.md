# Introduction

SoapsTraits is a trait system for RPG-style Minecraft servers. Each player has one active trait at a time. That trait provides:

1. **Base stats** - Permanent bonuses applied through MythicLib (attack damage, defense, mana, and more).
2. **Effects** - Event-driven abilities built from triggers, conditions, and actions in `traits.yml`.

## How it works

```
Player event (attack, damage, kill, etc.)
        |
        v
  Match trigger on active trait
        |
        v
  All conditions pass?  --no-->  skip
        |
       yes
        |
        v
  Cooldown ready?  --no-->  skip
        |
       yes
        |
        v
  Run actions (heal, damage bonus, particles, etc.)
```

Every player always has a trait. If MMOCore is installed and a trait is bound to their class, that trait is assigned automatically. Otherwise the server falls back to `default-trait` in `config.yml` (default: `human`).

## What you can configure

- **Class-linked traits** - Set `class: WARRIOR` on a trait to auto-assign when a player picks that MMOCore class.
- **Manual traits** - Leave out `class` and assign with `/sts set <player> <trait>`.
- **Combat effects** - Bonus damage on crit, dodge on sprint, execute windows, shield blocks, and more.
- **Passive tick effects** - Heal over time, regen pulses, or any action on a repeating timer.
- **In-game editor** - Staff with permission can create and edit traits through `/sts gui` without touching YAML.

## What SoapsTraits is not

- It is not a standalone class plugin. Class selection comes from MMOCore.
- It does not replace MythicLib stats. Base and temporary stat changes go through MythicLib.
- It does not add new mobs or skills. It reacts to vanilla combat events and MMOCore skill casts.

## Integration summary

| System | Role |
|--------|------|
| SoapsCommon | Shared config keys, startup, text parsing |
| MythicLib | Applies trait stat modifiers and temporary `add_stat` buffs |
| MMOCore | Class detection, mana, level, combat tag conditions |

See [Bridges and Integrations](Bridges-and-Integrations.md) for details.

## Next steps

1. [Getting Started](Getting-Started.md) - Install and verify on your server.
2. [Traits Configuration](Traits-Configuration.md) - Learn the YAML layout.
3. [Examples](Examples.md) - Study the bundled traits.
