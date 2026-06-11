# FAQ

Common questions from server owners running SoapsTraits 1.0.0.

## General

### Do players need a permission to have a trait?

No. Every player gets a trait automatically (class match or `default-trait`). Permissions only gate commands.

### Can a player have more than one trait?

No. One active trait per player. Use `/sts set` or class binding to change it.

### Does removing a trait leave the player with nothing?

No. `/sts remove` assigns `default-trait` from `config.yml` immediately.

## Classes and MMOCore

### Why did my player get `human` instead of their class trait?

Check:

1. MMOCore is installed and loaded.
2. The trait has `class: YOUR_CLASS_ID` matching MMOCore's profession ID.
3. The player joined after traits were configured (or relog to trigger `ensureTrait`).

### Does changing class update the trait?

Yes, when MMOCore fires a class change event and a trait is bound to the new class ID.

## Stats and combat

### Traits stats are in YAML but nothing changes in game

Install **MythicLib**. Base stats are applied through the MythicLib bridge.

### Does `/sts toggle` turn off stat bonuses?

No. Toggle disables **effects** (triggers, conditions, actions). Base `stats:` modifiers from the trait still apply.

### Why does my damage bonus not fire?

Check cooldown, conditions, and trigger type. Run `/sts debug <player>` while attacking. Common fixes:

- Add `has_target: true` for target-based conditions
- Lower `chance` for testing or remove it temporarily
- Confirm the player has toggle enabled

### Can I cancel environmental damage?

`cancel_event` on `damaged` can cancel the damage event. Environmental damage may not set a damager target.

## Configuration

### My effect disappeared after reload

With `strict-config-validation: true`, one bad condition or action drops the whole effect. Read console warnings on reload.

### What is the difference between `gui.enabled` and `settings.gui-enabled`?

- `gui.enabled` is the SoapsCommon suite master switch.
- `settings.gui-enabled` is the SoapsTraits-specific switch.

Both must be `true` for `/sts gui`.

### Can I use seconds instead of ticks?

Not directly. Convert: seconds times 20 = ticks. Example: 5 seconds = `100` ticks.

## Admin tools

### What does `/sts cooldown` do?

It saves a cooldown end timestamp in `playerdata.yml` for the named trait. Per-effect cooldowns in `traits.yml` control actual ability timing in combat.

### What does `/sts reset` do?

Sets the player back to `default-trait`, enables the trait, and clears cooldowns and runtime stats.

### Import did not work

Import files must:

- Be named `something.yml`
- Sit in `plugins/SoapsTraits/`
- Not use path characters like `/` or `..`

## Performance

### Will many tick effects lag my server?

Each online player evaluates tick effects every `tick-interval` ticks. Use cooldowns on all tick effects and keep the list reasonable per trait.

### How often is player data saved?

Every `save-interval` ticks (default 6000 = 5 minutes) and on shutdown.

## Related pages

- [Getting Started](Getting-Started.md)
- [Troubleshooting via debug](Getting-Started.md#enable-debug-output)
- [Commands and Permissions](Commands-and-Permissions.md)
