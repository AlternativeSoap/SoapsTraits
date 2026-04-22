# Getting Started

## Requirements

| Requirement | Details |
|-------------|---------|
| **Server Software** | Paper 1.21+ |
| **Java** | 25 or higher |
| **MythicLib** | 1.7.1+ |
| **MMOCore** | 1.13.1+ |

Both **MythicLib** and **MMOCore** are required. SoapsTraits will not load without them.

---

## Installation

1. Download and place **MythicLib** and **MMOCore** into your `plugins/` folder
2. Download `SoapsTraits.jar` and place it in `plugins/` as well
3. Start (or restart) your server
4. SoapsTraits will generate its config files:
   - `plugins/SoapsTraits/config.yml`
   - `plugins/SoapsTraits/traits.yml`
   - `plugins/SoapsTraits/messages.yml`

If everything loaded correctly, your console will show:
```
[SoapsTraits] MythicLib integration enabled.
[SoapsTraits] MMOCore integration enabled.
[SoapsTraits] Loaded 7 trait(s).
[SoapsTraits] SoapsTraits has been enabled!
```

A fourth file, `playerdata.yml`, is created automatically to store player trait assignments. You do not need to edit this file manually.

---

## Configuration Files

### config.yml
Controls global plugin settings.

```yaml
default-trait: human

settings:
  debug-mode: false
  tick-interval: 20
  save-interval: 6000
```

- `default-trait` — The trait assigned to players who don't match any class binding, or when a trait is removed. Must match a trait name in `traits.yml`.
- `debug-mode` — When `true`, prints trigger, condition, and action logs to the server console for all players. Useful for testing.
- `tick-interval` — How often the `tick` trigger fires, in ticks. `20` = once per second.
- `save-interval` — How often player data is auto-saved to disk, in ticks. `6000` = every 5 minutes. Data is always saved on shutdown and reload.

### traits.yml
This is where you define all your traits — their stats, effects, and class bindings. See [Traits Configuration](Traits-Configuration.md) for the full format.

### messages.yml
Every message the plugin sends to players or admins. You can change colors, wording, and formatting. Supports legacy color codes (`&a`), hex colors (`<#RRGGBB>`), and MiniMessage tags.

---

## Your First Trait

Here's a minimal trait to get you started:

```yaml
traits:
  warrior:
    class: WARRIOR
    stats:
      max_health: 8
      defense: 6

    effects:
      - trigger: kill
        actions:
          - heal: 3
          - send_message: "&aVictory!"
```

This trait:
- Is automatically assigned when a player picks the **WARRIOR** class in MMOCore
- Gives them **+8 max health** and **+6 defense** permanently
- Heals **3 HP** and shows a message whenever they kill an entity

After editing `traits.yml`, run `/sts reload` to apply the changes without restarting.

---

> **Next:** [Commands & Permissions →](Commands-and-Permissions.md)
