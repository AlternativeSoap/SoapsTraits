# GUI Editor

Staff can create and edit traits in-game without editing YAML by hand. The editor writes directly to `traits.yml` with backup protection.

## Requirements

All of the following must be true:

1. **SoapsCommon** suite GUI enabled:

```yaml
gui:
  enabled: true
```

2. **SoapsTraits** GUI enabled:

```yaml
settings:
  gui-enabled: true
```

3. Permission `soapstraits.user.gui` (default: op)

## Opening the editor

```
/sts gui
```

If the GUI is disabled, the player sees a "GUI disabled" message.

## Editor flow

### Trait list

- Shows all loaded traits, sorted alphabetically, paginated.
- Click a trait to open the **trait editor**.
- **Create** (emerald) starts a new trait with a generated unique name.

### Trait editor

From here you can:

- Rename the trait (validated: 3-32 chars, `a-z`, `0-9`, `_`, `-`)
- Set or clear the MMOCore `class` binding
- Open the **stats editor** to add or change base stats
- Open the **effects list** to add, edit, or remove effects
- Save, discard, or delete the trait

Saving updates `traits.yml` immediately and reapplies traits for online players. A backup copy is written to `traits.yml.bak` before each save.

### Effect editor

Per effect you can configure:

- **Trigger** - pick from all nine trigger types
- **Cooldown** - in ticks
- **Conditions** - add, edit, or remove condition entries
- **Actions** - add, edit, or remove action entries

The GUI exposes the same condition and action types documented in [Conditions](Conditions.md) and [Actions](Actions.md).

### Chat input

Some fields prompt for text in chat (rename, numeric values, messages). Type your value in chat or cancel through the GUI controls. Chat input is cancelled automatically on quit.

### Undo and redo

The editor keeps undo/redo stacks while you have a session open. Closing the GUI or quitting clears your session state.

## Click debounce

```yaml
settings:
  gui-click-debounce-ms: 150
```

Prevents accidental double-clicks from firing duplicate edits.

## Permissions

| Permission | Default | Access |
|------------|---------|--------|
| `soapstraits.user.gui` | op | Open and use the editor |

Players without this permission cannot open `/sts gui` even if they have other `soapstraits.user.*` nodes.

## When to use YAML instead

- Bulk edits across many traits
- Copy-paste from external sources
- Version control and review in Git

You can mix both workflows: edit in GUI, then fine-tune in YAML and `/sts reload`.

## Related pages

- [Traits Configuration](Traits-Configuration.md)
- [Import Export](Import-Export.md)
- [Commands and Permissions](Commands-and-Permissions.md)
