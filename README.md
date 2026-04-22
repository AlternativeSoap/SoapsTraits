# SoapsTraits Documentation

**SoapsTraits** is a trait system plugin for Paper servers that adds class-based passive stat bonuses and triggered combat abilities to your RPG. This wiki covers everything you need to install, configure, and customize it.

---

## Table of Contents

| Page | What's Inside |
|------|---------------|
| [Introduction](Introduction.md) | What SoapsTraits does and how it works |
| [Getting Started](Getting-Started.md) | Installation, dependencies, first setup |
| [Commands & Permissions](Commands-and-Permissions.md) | All commands and permission nodes |
| [Traits Configuration](Traits-Configuration.md) | Creating and configuring traits in `traits.yml` |
| [Triggers](Triggers.md) | When effects activate |
| [Conditions](Conditions.md) | Checks that must pass before actions run |
| [Actions](Actions.md) | What effects actually do |
| [Stats System](Stats-System.md) | Stat bonuses and how they apply |
| [Cooldowns & Durations](Cooldowns-and-Durations.md) | Time values and cooldown behavior |
| [Integrations](Bridges-and-Integrations.md) | How MythicLib and MMOCore connect |
| [Default Configs](Default-Configs.md) | Full default config, traits, and messages files |
| [Examples](Examples.md) | Ready-to-use trait examples |

---

## Quick Info

- **Version:** 1.0.0
- **Author:** AlternativeSoap
- **Server:** Paper 1.21+
- **Required plugins:** MythicLib 1.7.1+, MMOCore 1.13.1+
- **Website:** [soapsuniverse.com](http://www.soapsuniverse.com)

---

## Quick Start

1. Install **MythicLib** and **MMOCore** on your server
2. Drop `SoapsTraits.jar` into `plugins/`
3. Start the server — config files generate automatically
4. Edit `plugins/SoapsTraits/traits.yml` to set up your traits
5. Run `/sts reload` to apply your changes in-game
