# Introduction

## What is SoapsTraits?

SoapsTraits gives your players unique traits tied to their MMOCore class. Each trait can grant permanent stat bonuses and trigger special effects during combat — things like healing on kill, reducing damage when blocking, or boosting speed after landing a crit.

When a player selects the Warrior class in MMOCore, they automatically get the Warrior trait. When they switch classes, their trait switches too. You can also assign traits manually to any online player with a command.

---

## How Effects Work

Every trait effect follows the same logic: a **trigger** says when it can activate, **conditions** decide whether it actually fires, and **actions** define what happens.

```
Player does something (attacks, gets hit, kills an enemy...)
    ↓
Trigger matches — the effect wakes up
    ↓
Conditions are checked (health below 30%? 25% chance roll? sprinting?)
    ↓
All conditions pass → Actions execute (heal, damage bonus, particles...)
    ↓
Cooldown starts (if one is set)
```

**Example:** A Berserker trait might have an effect that says: whenever the player attacks (`trigger: attack`) and their health is below 30% (`health_below: 30%`), deal 30% extra damage (`damage_bonus: 30%`). This creates a natural "last stand" feeling — the lower your health, the harder you hit.

---

## What You Can Customize

- **Traits** — Create as many as you want. Each has a name, an optional class binding, stats, and effects.
- **Stats** — Permanent flat bonuses like max health, defense, speed, mana, crit chance, and more.
- **Effects** — Triggered abilities using 9 triggers, 18 conditions, and 12 actions.
- **Messages** — All plugin messages are customizable with color codes, hex colors, and MiniMessage.

---

## Requirements

| Dependency | Version |
|------------|---------|
| Paper | 1.21+ |
| Java | 25+ |
| MythicLib | 1.7.1+ |
| MMOCore | 1.13.1+ |

Both **MythicLib** and **MMOCore** are required. SoapsTraits will not load without them.

---

> **Next:** [Getting Started →](Getting-Started.md)
