# Bridges & Integrations

SoapsTraits requires two plugins to work: **MythicLib** and **MMOCore**. Both must be installed on your server. This page covers what each one contributes and what happens when specific features rely on them.

---

## MythicLib

MythicLib powers the stat system. SoapsTraits uses it to apply stat bonuses to players and to detect skill casts.

**What it enables:**

| Feature | Notes |
|---------|-------|
| Base stats (`stats:` block) | Permanent bonuses from a trait |
| `add_stat` action | Temporary stat buffs with a duration |
| `skill_cast` trigger | Fires when a player casts a MythicLib skill |
| `has_stat` condition | Checks if a player has a non-zero stat value |

**Stat name mapping:**

When you write stat names in `traits.yml`, SoapsTraits maps them to MythicLib's internal names:

| `traits.yml` name | MythicLib stat |
|-------------------|----------------|
| `attack_damage` | `ATTACK_DAMAGE` |
| `crit_chance` | `CRITICAL_STRIKE_CHANCE` |
| `crit_damage` | `CRITICAL_STRIKE_POWER` |
| `defense` | `DEFENSE` |
| `max_health` | `MAX_HEALTH` |
| `speed` | `MOVEMENT_SPEED` |
| `health_regen` | `HEALTH_REGENERATION` |
| `mana` | `MAX_MANA` |
| `mana_regen` | `MANA_REGENERATION` |
| `cooldown_reduction` | `COOLDOWN_REDUCTION` |

**If MythicLib is missing:** The plugin won't start — it's a required dependency. If the version you have doesn't support skill cast events, the `skill_cast` trigger simply won't fire; everything else works normally.

---

## MMOCore

MMOCore powers the class system, mana, combat tagging, and player levels.

**What it enables:**

| Feature | Notes |
|---------|-------|
| Class binding | Auto-assign a trait when a player selects an MMOCore class |
| `is_in_combat` condition | Checks MMOCore's combat tag |
| `give_mana` action | Restores a player's mana |
| `mana_above` / `mana_below` conditions | Checks mana percentage (requires MythicLib too) |
| `level_above` condition | Checks the player's MMOCore level |

**Class auto-assignment:** Add a `class:` line to a trait definition with the MMOCore class ID. When a player picks that class, they automatically receive that trait.

```yaml
warrior:
  class: WARRIOR    # must match the MMOCore class ID exactly (case-insensitive)
```

**If MMOCore is missing:** The plugin won't start — it's a required dependency. If specific MMOCore events aren't available in your version, those features (class auto-assignment) are silently skipped; the rest of the plugin works normally.

---

## Feature Summary

| Feature | Requires | If unavailable |
|---------|----------|----------------|
| `stats:` base modifiers | MythicLib | Silently ignored |
| `add_stat` action | MythicLib | Does nothing |
| `skill_cast` trigger | MythicLib | Never fires |
| `give_mana` action | MMOCore | Does nothing |
| `is_in_combat` condition | MMOCore | Always false |
| `mana_above` / `mana_below` | MMOCore + MythicLib | Always false |
| `level_above` condition | MMOCore | Always false |
| Class auto-assignment | MMOCore | Uses default trait |

---

> **Next:** [Default Configs →](Default-Configs.md)
