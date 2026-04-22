# Commands & Permissions

The main command is `/sts` (alias: `/soapstraits`).

| Command | Description | Permission |
|---------|-------------|------------|
| `/sts info` | View your current trait, stats, and effect count | `soapstraits.user` |
| `/sts set <player> <trait>` | Assign a trait to a player | `soapstraits.admin.set` |
| `/sts remove <player>` | Remove a player's trait (resets to default) | `soapstraits.admin.remove` |
| `/sts reload` | Reload all config files without restarting | `soapstraits.admin.reload` |
| `/sts list` | List all available traits | `soapstraits.admin.list` |
| `/sts debug <player>` | Toggle per-player debug output | `soapstraits.admin.debug` |

---

## Permissions

| Permission | Default | Description |
|------------|---------|-------------|
| `soapstraits.user` | Everyone | Allows use of `/sts info` |
| `soapstraits.admin.set` | OP | Assign traits to players |
| `soapstraits.admin.remove` | OP | Remove traits from players |
| `soapstraits.admin.reload` | OP | Reload configs |
| `soapstraits.admin.list` | OP | List all traits |
| `soapstraits.admin.debug` | OP | Toggle debug mode for a player |

---

## Command Details

### `/sts info`
Shows your currently active trait, its stat bonuses, and how many effects it has. Only works for players (not console).

Example output:
```
---------- Trait Info ----------
Active Trait: warrior
Stat Modifiers:
  - max_health: +8.0
  - defense: +6.0
  - attack_damage: +3.0
Configured Effects: 2
```

---

### `/sts set <player> <trait>`
Assigns a trait to an online player. The player's old trait stats are removed and the new ones applied immediately. Both you and the target player receive confirmation messages.

Trait names must match exactly what's defined in `traits.yml` (case-insensitive). Tab completion works for both player names and trait names.

```
/sts set Notch warrior
/sts set Steve mage
```

---

### `/sts remove <player>`
Removes a player's current trait. Since every player must always have a trait, the default trait (set in `config.yml`) is assigned automatically after removal.

```
/sts remove Notch
```

---

### `/sts reload`
Reloads `config.yml`, `traits.yml`, and `messages.yml`. Player trait assignments are preserved — only the trait definitions are refreshed. Active stats are cleaned up and re-applied to all online players.

Use this after making any changes to your config files instead of restarting the server.

---

### `/sts list`
Prints all trait names currently loaded from `traits.yml`. Useful for checking what's available before using `/sts set`.

---

### `/sts debug <player>`
Toggles real-time debug output for a specific player. When active, that player receives chat messages showing which triggers fire, which conditions pass or fail, and which actions execute — useful for testing new traits.

Run the command again on the same player to turn it off.

```
/sts debug Notch      # Enable debug for Notch
/sts debug Notch      # Disable debug for Notch
```

You can also enable global console debug output by setting `debug-mode: true` in `config.yml`.

---

## Permission Nodes

| Permission | Description | Default |
|------------|-------------|---------|
| `soapstraits.user` | Access to `/sts info` | `true` (all players) |
| `soapstraits.admin.set` | Set a player's trait | `op` |
| `soapstraits.admin.remove` | Remove a player's trait | `op` |
| `soapstraits.admin.reload` | Reload plugin configuration | `op` |
| `soapstraits.admin.list` | List all available traits | `op` |
| `soapstraits.admin.debug` | Toggle debug monitoring | `op` |

---

## Aliases

| Alias | Resolves To |
|-------|-------------|
| `/soapstraits` | `/sts` |

---

> **Next:** [Traits Configuration →](Traits-Configuration.md)
