# Getting Started

This guide walks you through installing SoapsTraits 1.0.0 and confirming it works on your server.

## Requirements

- **Paper 1.21** (or compatible fork)
- **SoapsCommon** installed and running
- **MythicLib** and **MMOCore** strongly recommended for full functionality

## Installation

1. Download **SoapsTraits 1.0.0** and place the JAR in your `plugins` folder.
2. Install **SoapsCommon** if you have not already.
3. Install **MythicLib** and **MMOCore** for class binding and stat support.
4. Start or restart the server.
5. Check the console for:
   - `Loaded X trait(s).`
   - `MythicLib integration enabled.` (if MythicLib is present)
   - `MMOCore integration enabled.` (if MMOCore is present)

On first run the plugin creates:

```
plugins/SoapsTraits/
  config.yml
  traits.yml
  messages.yml
  playerdata.yml
```

## First configuration check

Open `plugins/SoapsTraits/config.yml` and confirm:

```yaml
default-trait: human
```

This is the trait assigned when no class match exists. The bundled `traits.yml` includes `human` plus one trait per MMOCore class.

## Assign a trait manually

For testing without MMOCore:

```
/sts set <player> warrior
```

The player should see a chat message confirming the assignment. Base stats apply within a few ticks of join or assignment.

## Verify trait info

As the player:

```
/sts info
```

This shows the active trait name, stat modifiers, and how many effects are configured.

## Reload after edits

After changing `traits.yml` or `config.yml`:

```
/sts reload
```

Console warnings point to invalid triggers, conditions, or actions. With `strict-config-validation: true` (default), a bad condition or action causes the **entire effect** to be skipped at load time.

## Enable debug output

For troubleshooting effect firing:

1. Set `debug-mode: true` in `config.yml` and reload.
2. Run `/sts debug <player>` to toggle live debug messages in that player's chat.

Debug lines show triggers, condition results, and actions as they run.

## Enable the GUI editor

The trait editor requires:

- `gui.enabled: true` in `config.yml` (suite master switch)
- `settings.gui-enabled: true` in `config.yml`
- Permission `soapstraits.user.gui` (default: op)

Then run:

```
/sts gui
```

See [GUI Editor](GUI-Editor.md) for the full workflow.

## Common first-time issues

| Problem | Fix |
|---------|-----|
| Plugin will not enable | Install SoapsCommon first |
| Stats do not change | Install MythicLib; rejoin or `/sts reload` |
| Class traits not assigned | Confirm MMOCore is loaded and `class:` matches the class ID |
| Effects never fire | Check cooldowns, conditions, and `/sts debug` |
| GUI says disabled | Set both `gui.enabled` and `settings.gui-enabled` to true |

## Next steps

- [Traits Configuration](Traits-Configuration.md) - Edit `traits.yml`
- [Commands and Permissions](Commands-and-Permissions.md) - Full `/sts` list
- [Examples](Examples.md) - Bundled trait breakdowns
