# Commands and Permissions

Main command: **`/sts`**  
Alias: **`/soapstraits`**

Run `/sts` with no arguments to print the full usage list from `messages.yml`.

## Player commands

| Command | Permission | Default | Description |
|---------|------------|---------|-------------|
| `/sts info` | `soapstraits.user` | true | Show your active trait, stats, and effect count |
| `/sts toggle` | `soapstraits.user.toggle` | true | Enable or disable your trait's **effects** (base stats stay) |
| `/sts stats` | `soapstraits.user.stats` | true | View your runtime trait stats |
| `/sts gui` | `soapstraits.user.gui` | op | Open the in-game trait editor |

`info`, `toggle`, and `gui` require the sender to be a player.

## Admin commands

| Command | Permission | Default | Description |
|---------|------------|---------|-------------|
| `/sts set <player> <trait>` | `soapstraits.admin.set` | op | Assign a trait to an online player |
| `/sts remove <player>` | `soapstraits.admin.remove` | op | Remove trait and assign `default-trait` |
| `/sts reload` | `soapstraits.admin.reload` | op | Reload config, traits, and messages |
| `/sts list` | `soapstraits.admin.list` | op | List all trait IDs |
| `/sts debug <player>` | `soapstraits.admin.debug` | op | Toggle live effect debug for a player |
| `/sts reset <player>` | `soapstraits.admin.reset` | op | Reset player data to default trait |
| `/sts stats <player>` | `soapstraits.admin.stats` | op | View another player's runtime stats |
| `/sts migrate <old> <new>` | `soapstraits.admin.migrate` | op | Move all players from one trait to another |
| `/sts cooldown <player> <trait> <ticks>` | `soapstraits.admin.cooldown` | op | Store cooldown timestamp in playerdata |
| `/sts version` | `soapstraits.admin.version` | op | Show plugin version (1.0.0) |
| `/sts test <trait> [player]` | `soapstraits.admin.test` | op | Assign trait for testing (defaults to self) |
| `/sts import <file.yml>` | `soapstraits.admin.import` | op | Replace traits.yml and reload |
| `/sts export [file.yml]` | `soapstraits.admin.export` | op | Copy traits.yml to a backup file |

### Command notes

- **set** requires the target player to be online.
- **remove** never leaves a player without a trait; default is assigned immediately.
- **migrate** updates saved assignments only; online players keep playing until relog or set.
- **test** is shorthand for `set` during balancing sessions.
- **reload** reports merged default config keys from the JAR when new keys were added.

## Permission tree

```
soapstraits.user                    # Basic access (info)
soapstraits.user.stats              # /sts stats (self)
soapstraits.user.toggle             # /sts toggle
soapstraits.user.gui                # /sts gui

soapstraits.admin.set
soapstraits.admin.remove
soapstraits.admin.reload
soapstraits.admin.list
soapstraits.admin.debug
soapstraits.admin.reset
soapstraits.admin.stats             # /sts stats <other>
soapstraits.admin.migrate
soapstraits.admin.cooldown
soapstraits.admin.version
soapstraits.admin.test
soapstraits.admin.import
soapstraits.admin.export
```

There is no parent `soapstraits.admin` node in `plugin.yml`. Grant each admin node explicitly or use a permissions plugin group.

## Tab completion

Tab completion filters by permission:

- Subcommands you cannot use are hidden.
- Player names for `set`, `remove`, `debug`, `reset`, `stats`, `cooldown`, `test`.
- Trait IDs for `set`, `migrate`, `cooldown`, `test`.

## LuckPerms example

```yaml
# Default players
- soapstraits.user
- soapstraits.user.stats
- soapstraits.user.toggle

# Moderators
- soapstraits.admin.list
- soapstraits.admin.debug
- soapstraits.admin.stats

# Admins
- soapstraits.admin.*
# (grant each node individually; no wildcard in plugin.yml)
```

## Related pages

- [Getting Started](Getting-Started.md)
- [GUI Editor](GUI-Editor.md)
- [Import Export](Import-Export.md)
