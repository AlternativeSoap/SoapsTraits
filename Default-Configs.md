# Default Configs

SoapsTraits ships three main config files. On first run they are copied to `plugins/SoapsTraits/`. On `/sts reload`, missing keys from the JAR are merged into your live files without overwriting your values.

## config.yml

```yaml
default-trait: human

settings:
  debug-mode: false
  strict-config-validation: true
  tick-interval: 20
  save-interval: 6000
  max-cooldown-ticks: 72000
  gui-enabled: true
  gui-click-debounce-ms: 150

gui:
  enabled: true
```

| Key | Default | Description |
|-----|---------|-------------|
| `default-trait` | `human` | Trait assigned when no class match exists |
| `settings.debug-mode` | `false` | Log trigger/condition/action details to console |
| `settings.strict-config-validation` | `true` | Skip entire effects with invalid conditions/actions |
| `settings.tick-interval` | `20` | How often `tick` triggers are evaluated (ticks) |
| `settings.save-interval` | `6000` | Auto-save `playerdata.yml` interval (ticks, 6000 = 5 min) |
| `settings.max-cooldown-ticks` | `72000` | Hard cap for effect cooldown values |
| `settings.gui-enabled` | `true` | SoapsTraits GUI toggle |
| `settings.gui-click-debounce-ms` | `150` | GUI click cooldown |
| `gui.enabled` | `true` | SoapsCommon suite master GUI switch |

Both `gui.enabled` and `settings.gui-enabled` must be true for `/sts gui`.

## traits.yml

Contains all trait definitions under `traits:`. See [Traits Configuration](Traits-Configuration.md).

Bundled content (1.0.0):

- 7 class-bound traits: `human`, `warrior`, `mage`, `rogue`, `paladin`, `archer`, `berserker`
- 3 showcase traits without class binding
- Inline comments documenting triggers, conditions, actions, and balance ranges

The file header documents the full schema for quick reference while editing.

## messages.yml

All player-facing text: command output, errors, GUI titles, button labels, debug lines.

Uses MiniMessage gradients with consistent color meanings:

| Meaning | Gradient |
|---------|----------|
| Success | green to teal |
| Error | red to orange |
| Warning | amber to orange |
| Info | blue to purple |
| Headers | gold to purple |

Legacy `&` color codes still work where parsed through SoapsCommon text utilities.

Edit `messages.yml` to match your server's tone. Reload with `/sts reload`.

## playerdata.yml

Auto-generated. Do not hand-edit unless you know the format.

```yaml
<uuid>:
  trait: warrior
  enabled: true
  cooldowns: {}
  runtime-stats: {}
```

- Created on first trait assignment
- Saved on interval (`save-interval`) and on shutdown
- Invalid trait names fall back to `default-trait` on load

## JAR default merge

On enable and reload, SoapsTraits merges new default keys from the embedded YAML into your files. `/sts reload` reports how many keys were merged per file. Your existing values are never overwritten.

## Related pages

- [Getting Started](Getting-Started.md)
- [Traits Configuration](Traits-Configuration.md)
- [GUI Editor](GUI-Editor.md)
