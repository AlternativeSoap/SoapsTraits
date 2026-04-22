# Examples

Ready-to-use trait configurations for a variety of RPG archetypes. Copy and adapt these for your own server.

---

## Basic Traits

### Starter (Human)
A balanced starter class with a small survival heal.

```yaml
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
```

---

### Warrior (Frontliner)
High HP and defense, gains a temporary defense buff when hit and heals on kill.

```yaml
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
```

---

### Mage (Spellcaster)
Mana-focused, recovers mana on skill cast and has a mana shield.

```yaml
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
```

---

### Rogue (Assassin)
Backstab bonus, adrenaline on crits, and a sprint dodge.

```yaml
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
```

---

### Paladin (Holy Tank)
Block-based damage reduction, heal on kill, and a full-health smite bonus.

```yaml
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
        - send_message: "&fHoly light restores you."

    - trigger: attack
      conditions:
        - health_above: 90%
      actions:
        - damage_bonus: 10%
```

---

### Archer (Ranged)
Bonus damage from distance, speed after kills, and marks crits with glowing.

```yaml
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
```

---

### Berserker (Rage)
Scales up with low HP — more damage and defense the closer to death.

```yaml
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

    - trigger: kill
      conditions:
        - health_below: 50%
      actions:
        - heal: 4
        - send_message: "&cYou feed on the kill."
```

---

## Advanced Examples

### Shadow Knight (Stealth Assassin)
A sneaky melee fighter with an invisibility cloak and backstab burst.

```yaml
shadow_knight:
  class: SHADOW_KNIGHT
  stats:
    crit_chance: 8
    crit_damage: 10
    attack_damage: 4
    speed: 0.02
  effects:
    # Crouch for invisibility
    - trigger: crouch
      cooldown: 400
      actions:
        - apply_potion_effect:
            type: INVISIBILITY
            duration: 100
            level: 1
        - send_message: "&7You vanish into the shadows..."
        - play_sound: ENTITY_ENDERMAN_TELEPORT

    # Massive backstab from stealth
    - trigger: attack
      conditions:
        - behind_target: true
        - has_potion_effect: INVISIBILITY
      actions:
        - damage_bonus: 75%
        - remove_potion_effect: INVISIBILITY
        - spawn_particle:
            type: SMOKE
            amount: 20
            spread: 0.5
            speed: 0.05
        - send_message: "&8Shadow Strike!"

    # Lifesteal on kill
    - trigger: kill
      actions:
        - heal: 4
        - spawn_particle:
            type: SOUL_FIRE_FLAME
            amount: 8
            spread: 0.4
            speed: 0.02
```

---

### Necromancer (Dark Mage)
Saps life on skill casts and grows more powerful at night.

```yaml
necromancer:
  class: NECROMANCER
  stats:
    mana: 30
    mana_regen: 3
    attack_damage: 2
  effects:
    # Drain life on skill cast at night
    - trigger: skill_cast
      cooldown: 80
      conditions:
        - time: NIGHT
        - chance: 40%
      actions:
        - heal: 3
        - give_mana: 10
        - spawn_particle:
            type: SOUL_FIRE_FLAME
            amount: 12
            spread: 0.5
            speed: 0.05
        - send_message: "&5Soul drain!"

    # Bone shield at low HP
    - trigger: damaged
      cooldown: 300
      conditions:
        - health_below: 35%
      actions:
        - add_stat:
            defense: 18
            duration: 120
        - apply_potion_effect:
            type: REGENERATION
            duration: 60
            level: 1
        - spawn_particle:
            type: CLOUD
            amount: 15
            spread: 0.6
        - send_message: "&fBone shield!"

    # Undead buff in the Nether
    - trigger: tick
      cooldown: 200
      conditions:
        - world: world_nether
      actions:
        - heal: 2
        - give_mana: 5
```

---

### Ice Mage (Control Mage)
Slows enemies and punishes them for attacking you.

```yaml
ice_mage:
  class: ICE_MAGE
  stats:
    mana: 25
    mana_regen: 2
    cooldown_reduction: 8
  effects:
    # Frost barrier: brief resistance when hit
    - trigger: damaged
      cooldown: 60
      actions:
        - apply_potion_effect:
            type: RESISTANCE
            duration: 40
            level: 1
        - spawn_particle:
            type: SNOWFLAKE
            amount: 15
            spread: 0.5
            speed: 0.05
        - send_message: "&bFrost barrier!"

    # Mana restore on skill use
    - trigger: skill_cast
      cooldown: 40
      conditions:
        - mana_below: 50%
      actions:
        - give_mana: 20
        - spawn_particle:
            type: ENCHANT
            amount: 10
            spread: 0.4

    # Blizzard: area damage bonus in cold biomes
    - trigger: attack
      conditions:
        - biome: snowy_plains
      actions:
        - damage_bonus: 20%
        - spawn_particle:
            type: SNOWFLAKE
            amount: 8
            spread: 0.3
            speed: 0.01
```

---

### Druid (Nature Healer)
Regenerates in forests, heals allies (self), and grows stronger in natural biomes.

```yaml
druid:
  class: DRUID
  stats:
    max_health: 4
    health_regen: 2
    mana_regen: 1.5
  effects:
    # Nature's embrace: passive healing in forest biomes
    - trigger: tick
      cooldown: 100
      conditions:
        - biome: forest
        - health_below: 80%
      actions:
        - heal: 3
        - spawn_particle:
            type: HAPPY_VILLAGER
            amount: 5
            spread: 0.4

    # Thorns: counter-damage on hit (via stat buff)
    - trigger: damaged
      cooldown: 80
      conditions:
        - chance: 25%
      actions:
        - add_stat:
            defense: 8
            health_regen: 1
            duration: 60
        - spawn_particle:
            type: COMPOSTER
            amount: 10
            spread: 0.4
        - send_message: "&2Nature's guard!"

    # Regrowth on kill (high-level players only)
    - trigger: kill
      conditions:
        - level_above: 15
      actions:
        - apply_potion_effect:
            type: REGENERATION
            duration: 80
            level: 2
        - heal: 5
        - send_message: "&aVitality restored."
```

---

## Tips for Building Traits

1. **Start simple** — add one effect at a time and test with `/sts debug`
2. **Balance cooldowns** — powerful effects should have longer cooldowns
3. **Use chance conditions** — prevent effects from feeling too deterministic
4. **Layer conditions** — combine 2–3 conditions for nuanced, skillful effects
5. **Use particles sparingly** — too many particles can be distracting/laggy
6. **Test mana conditions** — make sure MMOCore + MythicLib are both working
7. **Check biome/world names** — use exact Bukkit enum names (case-insensitive for biomes)

---

> **Back to start:** [Introduction →](Introduction.md)
