# SoapsTraits Wiki

**Version:** 1.0.0  
**Author:** AlternativeSoap  
**Minecraft API:** 1.21 (Paper)

SoapsTraits gives each player a class-style trait with passive stat bonuses and configurable combat effects. Traits are defined in YAML, assigned automatically from MMOCore classes or manually with commands, and edited in-game through a GUI.

## Requirements

| Plugin | Required |
|--------|----------|
| [SoapsCommon](https://www.soapsuniverse.com) | Yes (hard dependency) |
| MythicLib | Recommended (stat modifiers) |
| MMOCore | Recommended (class binding, mana, combat tag) |

## Quick links

- [Introduction](Introduction.md) - What the plugin does and how it fits your server
- [Getting Started](Getting-Started.md) - Install, first reload, assign a trait
- [Traits Configuration](Traits-Configuration.md) - Full `traits.yml` schema
- [Triggers](Triggers.md) - When effects run
- [Conditions](Conditions.md) - Rules that must pass
- [Actions](Actions.md) - What happens when an effect fires
- [Commands and Permissions](Commands-and-Permissions.md) - `/sts` reference
- [Examples](Examples.md) - Copy-paste trait ideas

## Main command

```
/sts
```

Alias: `/soapstraits`

Run `/sts` with no arguments to see the full usage list in chat.

## Config files

| File | Purpose |
|------|---------|
| `config.yml` | Server settings, default trait, tick interval |
| `traits.yml` | All trait definitions |
| `messages.yml` | Chat and GUI text |
| `playerdata.yml` | Per-player trait assignments (auto-managed) |

## Support

Docs in this folder match **SoapsTraits 1.0.0**. After updates, run `/sts reload` and check the console for config warnings.
