# Import and Export

SoapsTraits can copy `traits.yml` to or from other files in the plugin data folder. Useful for staging configs, sharing trait packs, or rolling back changes.

## File location

All import and export files must live in:

```
plugins/SoapsTraits/
```

## Security rules

File names must:

- End with `.yml`
- Not contain `..`, `/`, or `\`
- Resolve inside the plugin data folder (path traversal is blocked)

Invalid names fail with a console warning and no file change.

## Export

```
/sts export
/sts export my_traits.yml
```

Permission: `soapstraits.admin.export`

- Default output file: `traits_export.yml`
- Copies the current `traits.yml` to the named file
- Reports how many traits were exported
- Does **not** reload the plugin

## Import

```
/sts import backup_traits.yml
```

Permission: `soapstraits.admin.import`

1. Copies the named file **over** `traits.yml` (full replace)
2. Reloads the plugin automatically
3. Reports how many traits were found in the imported file

If the file is missing or invalid, import fails and `traits.yml` is unchanged.

## Suggested workflow

**Before a major edit:**

```
/sts export traits_backup_2026-06-11.yml
```

**Restore a backup:**

```
/sts import traits_backup_2026-06-11.yml
```

**Share traits between servers:**

1. Export on server A.
2. Copy the `.yml` file into server B's `plugins/SoapsTraits/` folder.
3. Run `/sts import <filename>` on server B.

## GUI saves vs import

The GUI saves individual trait changes to `traits.yml` with a local `traits.yml.bak`. Import replaces the entire file in one step. Prefer export/import for full snapshots.

## Related pages

- [Traits Configuration](Traits-Configuration.md)
- [GUI Editor](GUI-Editor.md)
- [Default Configs](Default-Configs.md)
