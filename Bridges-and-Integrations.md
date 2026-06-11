# Bridges and Integrations

SoapsTraits is built to sit alongside the Soaps suite and popular MMO plugins.

## SoapsCommon (required)

Listed as a hard dependency in `plugin.yml`.

Provides:

- Shared startup banner and config merge on reload
- `SoapsConfigKeys` for suite-wide settings like `gui.enabled`
- Text parsing for messages (MiniMessage and legacy `&` codes)

Without SoapsCommon the plugin will not enable.

## MythicLib (soft depend)

**Recommended.** Powers all stat modifier features.

### What it does

| Feature | Bridge behavior |
|---------|-----------------|
| Trait `stats:` block | Registers base modifiers on assign/reload |
| `add_stat` action | Temporary modifiers with optional duration |
| `has_stat` condition | Reads MythicLib stat totals |

### Stat name mapping

SoapsTraits YAML keys map to MythicLib stats automatically. See [Stats System](Stats-System.md).

### Without MythicLib

Console warns: `MythicLib not found. Stat modifier features will be disabled.`

Effects that only use heal, particles, sounds, etc. still work.

## MMOCore (soft depend)

**Recommended** for class-based servers.

### What it does

| Feature | Bridge behavior |
|---------|-----------------|
| `class:` on traits | Auto-assign trait when player picks that class |
| `skill_cast` trigger | Listens for MMOCore skill cast events |
| `give_mana` action | Restores mana through MMOCore API |
| `mana_above` / `mana_below` | Reads current mana vs max mana percent |
| `level_above` | Reads MMOCore player level |
| `is_in_combat` | Reads MMOCore combat tag |

### Class binding

```yaml
mage:
  class: MAGE
```

The value must match `PlayerData.getProfess().getId()` from MMOCore (example: `WARRIOR`, `MAGE`, `HUMAN`).

On join and class change, `ensureTrait()` picks the matching trait or falls back to `default-trait`.

### Reflective listeners

Skill cast and class change events are registered reflectively so the plugin JAR loads even when MMOCore is absent. If MMOCore is not installed, those triggers and conditions simply do nothing or fail closed.

## Plugin load order

From `plugin.yml`:

```yaml
depend:
  - SoapsCommon
softdepend:
  - MythicLib
  - MMOCore
```

SoapsCommon always loads first. MythicLib and MMOCore should be present at startup for clean integration.

## Paper API

- `api-version: '1.21'`
- Built against Paper 1.21.11 API
- Combat listeners use standard Bukkit events (`EntityDamageByEntityEvent`, etc.)

## What is not integrated (1.0.0)

- PlaceholderAPI placeholders (use `/sts info` or custom plugins)
- Vault economy
- WorldGuard regions (use `world` condition for coarse filtering)
- Custom mob plugins (use `target_type` for vanilla entity types)

## Related pages

- [Getting Started](Getting-Started.md)
- [Stats System](Stats-System.md)
- [Conditions](Conditions.md)
