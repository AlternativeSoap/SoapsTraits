# Default Configs

These are the full default configuration files that SoapsTraits generates on first startup. Use them as a reference when building your own setup.

---

## config.yml

Located at `plugins/SoapsTraits/config.yml`

```yaml
# SoapsTraits Configuration
# ============================================================

# Fallback trait for players who don't have a class-bound trait.
# If a trait has 'class: WARRIOR' in traits.yml and the player picks
# the Warrior class, that trait is assigned automatically.
# This default is only used when no class match is found.
default-trait: human

settings:
  # Print debug info to console (trigger/condition/action logs)
  debug-mode: false

  # How often passive (tick) effects run, in ticks. (20 = 1 second)
  tick-interval: 20

  # Auto-save player data interval in ticks. (6000 = 5 minutes)
  save-interval: 6000
```

---

## messages.yml

Located at `plugins/SoapsTraits/messages.yml`

```yaml
# SoapsTraits Messages
# Supports: HEX colors (<#RRGGBB>), legacy colors (&a, &l, etc.), MiniMessage tags

prefix: "&8[&6SoapsTraits&8] "

# === Trait Assignment ===
trait-assigned: "&aYou have been assigned the &e{trait} &atrait!"
trait-removed: "&cYour trait has been removed."
trait-changed: "&eYour trait has been changed to &6{trait}&e."
trait-default-assigned: "&7You have been assigned the default trait: &e{trait}"
trait-fallback-assigned: "&7Your previous trait was invalid. Assigned default: &e{trait}"

# === Permissions ===
no-permission: "&cYou do not have permission to use this command."

# === Errors ===
trait-not-found: "&cTrait &e{trait} &cnot found."
player-not-found: "&cPlayer &e{player} &cnot found."
player-no-trait: "&e{player} &cdoes not have a trait."

# === Info Command ===
trait-info-header: "&6&m----------&r &6Trait Info &6&m----------"
trait-info-trait: "&7Active Trait: &e{trait}"
trait-info-stats-header: "&7Stat Modifiers:"
trait-info-stat-entry: "&8  - &f{stat}: &a+{value}"
trait-info-no-stats: "&8  None"
trait-info-effects: "&7Configured Effects: &e{count}"
trait-info-none: "&7You do not have a trait assigned."

# === Admin Commands ===
trait-set-success: "&aSet &e{player}&a's trait to &e{trait}&a."
trait-remove-success: "&aRemoved &e{player}&a's trait. Default trait &e{default}&a assigned."
trait-already-assigned: "&e{player} &aalready has the &e{trait} &atrait."
reload-success: "&aPlugin reloaded successfully!"

# === List Command ===
trait-list-header: "&6&m----------&r &6Available Traits &6&m----------"
trait-list-entry: "&e  - {trait}"

# === Debug Command ===
debug-toggle-on: "&aDebug monitoring &2enabled &afor &e{player}&a."
debug-toggle-off: "&cDebug monitoring &4disabled &cfor &e{player}&c."
debug-header: "&6&m----------&r &6Debug: &e{player} &6&m----------"
debug-trait: "&7Active Trait: &e{trait}"
debug-stats: "&7Base Stats: &e{stats}"
debug-effects: "&7Configured Effects: &e{count}"

# === Usage ===
command-usage: "&cUsage: /sts <info|set|remove|reload|list|debug>"
```

### Message Placeholders

| Placeholder | Used In | Value |
|-------------|---------|-------|
| `{trait}` | trait-assigned, trait-changed, etc. | Trait name |
| `{player}` | trait-set-success, player-not-found, etc. | Player name |
| `{stat}` | trait-info-stat-entry | Stat key name |
| `{value}` | trait-info-stat-entry | Stat value |
| `{count}` | trait-info-effects, debug-effects | Number of effects |
| `{stats}` | debug-stats | Formatted stat list |
| `{default}` | trait-remove-success | Default trait name |

---

## traits.yml

Located at `plugins/SoapsTraits/traits.yml`

```yaml
# ============================================================
#  SoapsTraits - Trait Configuration
# ============================================================

traits:

  # ---------------------------------------------------------
  #  HUMAN - Starter class, well-rounded
  # ---------------------------------------------------------
  human:
    class: HUMAN
    stats:
      max_health: 2
      health_regen: 0.5
    effects:
      - trigger: damaged
        cooldown: 200
        conditions:
          - health_below: 25%
        actions:
          - heal: 2
          - send_message: "&eSurvival instincts kick in..."

  # ---------------------------------------------------------
  #  WARRIOR - Frontline bruiser, high HP and armor
  # ---------------------------------------------------------
  warrior:
    class: WARRIOR
    stats:
      max_health: 8
      defense: 6
      attack_damage: 3
    effects:
      - trigger: damaged
        cooldown: 100
        conditions:
          - chance: 20%
        actions:
          - add_stat:
              defense: 12
              duration: 80
          - play_sound: ITEM_ARMOR_EQUIP_IRON
          - spawn_particle:
              type: CRIT
              amount: 8
              spread: 0.4
              speed: 0.02
      - trigger: kill
        cooldown: 60
        conditions:
          - chance: 30%
        actions:
          - heal: 3
          - send_message: "&6For glory!"

  # ---------------------------------------------------------
  #  MAGE - Spell-focused, mana regen and cooldowns
  # ---------------------------------------------------------
  mage:
    class: MAGE
    stats:
      mana: 20
      mana_regen: 2
      cooldown_reduction: 5
    effects:
      - trigger: skill_cast
        cooldown: 60
        conditions:
          - chance: 25%
        actions:
          - give_mana: 15
          - spawn_particle:
              type: ENCHANT
              amount: 20
              spread: 0.6
              speed: 0.1
          - send_message: "&bArcane surge!"
      - trigger: damaged
        conditions:
          - mana_above: 60%
        actions:
          - reduce_damage: 15%

  # ---------------------------------------------------------
  #  ROGUE - Fast and sneaky, crit-focused
  # ---------------------------------------------------------
  rogue:
    class: ROGUE
    stats:
      crit_chance: 10
      crit_damage: 5
      speed: 0.03
    effects:
      - trigger: attack
        conditions:
          - behind_target: true
        actions:
          - damage_bonus: 35%
          - spawn_particle:
              type: SMOKE
              amount: 12
              spread: 0.3
              speed: 0.01
      - trigger: crit
        cooldown: 100
        actions:
          - apply_potion_effect:
              type: SPEED
              duration: 60
              level: 2
          - send_message: "&eAdrenaline rush!"
      - trigger: damaged
        conditions:
          - is_sprinting: true
          - chance: 12%
        actions:
          - cancel_event: true
          - send_message: "&7Dodged!"
          - play_sound: ENTITY_PHANTOM_FLAP

  # ---------------------------------------------------------
  #  PALADIN - Holy tank, self-healing and damage reduction
  # ---------------------------------------------------------
  paladin:
    class: PALADIN
    stats:
      max_health: 6
      defense: 8
      health_regen: 1
    effects:
      - trigger: damaged
        cooldown: 80
        conditions:
          - is_blocking: true
        actions:
          - reduce_damage: 40%
          - spawn_particle:
              type: END_ROD
              amount: 10
              spread: 0.5
              speed: 0.03
          - play_sound: BLOCK_ANVIL_LAND
      - trigger: kill
        conditions:
          - chance: 35%
        actions:
          - heal: 5
          - spawn_particle:
              type: HEART
              amount: 4
              spread: 0.4
              speed: 0
          - send_message: "&fHoly light restores you."
      - trigger: attack
        conditions:
          - health_above: 90%
        actions:
          - damage_bonus: 10%

  # ---------------------------------------------------------
  #  ARCHER - Ranged fighter, distance bonuses
  # ---------------------------------------------------------
  archer:
    class: ARCHER
    stats:
      attack_damage: 4
      crit_chance: 5
      speed: 0.02
    effects:
      - trigger: attack
        conditions:
          - distance_to_target: 15
        actions:
          - damage_bonus: 20%
          - play_sound: ENTITY_ARROW_HIT_PLAYER
      - trigger: kill
        cooldown: 80
        actions:
          - apply_potion_effect:
              type: SPEED
              duration: 80
              level: 1
          - send_message: "&aQuick feet!"
      - trigger: crit
        cooldown: 60
        conditions:
          - chance: 30%
        actions:
          - apply_potion_effect:
              type: GLOWING
              duration: 60
              level: 1

  # ---------------------------------------------------------
  #  BERSERKER - Gets stronger the closer to death
  # ---------------------------------------------------------
  berserker:
    stats:
      attack_damage: 6
    effects:
      - trigger: attack
        conditions:
          - health_below: 30%
        actions:
          - damage_bonus: 30%
          - spawn_particle:
              type: DAMAGE_INDICATOR
              amount: 5
              spread: 0.3
              speed: 0.02
      - trigger: damaged
        cooldown: 200
        conditions:
          - health_below: 20%
        actions:
          - add_stat:
              defense: 20
              duration: 100
          - send_message: "&4&lBlood rage!"
          - play_sound: ENTITY_ENDER_DRAGON_GROWL
          - spawn_particle:
              type: LAVA
              amount: 10
              spread: 0.5
              speed: 0.01
      - trigger: kill
        conditions:
          - health_below: 50%
        actions:
          - heal: 4
          - send_message: "&cYou feed on the kill."
```

---

> **Next:** [Examples →](Examples.md)
