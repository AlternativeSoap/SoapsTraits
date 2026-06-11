# Changelog

## 1.0.0

Initial public release of SoapsTraits.

### Core

- Trait system with YAML configuration (`traits.yml`)
- One trait per player with automatic class binding via MMOCore
- Fallback `default-trait` when no class match exists
- Base stat modifiers through MythicLib
- Effect engine with triggers, conditions, actions, and per-effect cooldowns
- Player data persistence (`playerdata.yml`) with auto-save
- Strict config validation option for safer YAML editing

### Triggers (9)

`attack`, `damaged`, `kill`, `crit`, `skill_cast`, `tick`, `death`, `respawn`, `crouch`

### Conditions (23)

`health_below`, `health_above`, `health_between`, `chance`, `biome`, `time`, `distance_to_target`, `behind_target`, `has_stat`, `is_sprinting`, `is_sneaking`, `is_blocking`, `is_in_combat`, `has_potion_effect`, `target_type`, `has_target`, `target_is_player`, `target_health_below`, `world`, `mana_above`, `mana_below`, `level_above`, `weather`

### Actions (16)

`damage_bonus`, `reduce_damage`, `heal`, `add_stat`, `apply_potion_effect`, `remove_potion_effect`, `send_message`, `play_sound`, `spawn_particle`, `set_on_fire`, `give_mana`, `cancel_event`, `feed`, `knockback_target`, `teleport_forward`, `clear_negative_effects`

### Commands

`/sts` (alias `/soapstraits`) with subcommands: `info`, `set`, `remove`, `reload`, `list`, `debug`, `reset`, `toggle`, `stats`, `migrate`, `cooldown`, `version`, `test`, `import`, `export`, `gui`

### GUI

- In-game trait list, trait editor, stats editor, and effect editor
- Undo/redo support during editing sessions
- Safe YAML save with `traits.yml.bak` backup

### Bundled traits

- Class traits: Human, Warrior, Mage, Rogue, Paladin, Archer, Berserker
- Showcase traits: Shadowstep, Executioner, Guardian

### Dependencies

- Required: SoapsCommon
- Soft: MythicLib, MMOCore
- Paper API 1.21
