# Handcuffs

Handcuffs let you restrain a downed player, lock away their inventory, and keep them captive for as long as the cuffs hold.

## Crafting

Handcuffs are crafted **shapeless** using the ingredients defined in config (default: 3 iron ingots + 2 string). The recipe is injected at server load — no datapack required.

## Applying Cuffs

**Right-click** a downed, uncuffed player while holding the **Handcuffs** item.

- The handcuff item is consumed.
- The target's entire inventory is cleared and saved to disk.
- You receive a **[Handcuff Key](#keys)** for that prisoner.

The target does not need to still be in the invincibility window — any downed player can be cuffed.

## Restrictions on a Cuffed Player

While cuffed, a player:

- Cannot sprint
- Cannot attack entities
- Cannot interact with blocks or entities (no right-clicking)
- Cannot use items (no eating, throwing, etc.)
- Cannot mine blocks
- Has their inventory continuously emptied — any item that enters it is immediately dropped on the ground
- Cannot use `/surrender`

These restrictions are enforced server-side; no client mod is needed.

## Keys

When cuffs are applied, the cuffing player receives a **Handcuff Key** item (base item configurable via `keyItemId`).

- The key's lore shows the prisoner's name and UUID.
- The key stores the prisoner's UUID in its item NBT (`ivanica_key_for` tag).
- **Right-clicking a cuffed player with their key** uncuffs them and consumes the key.
- Using the **wrong key** on a player sends an error message and does nothing.

Keys are unique — they cannot be duplicated by any vanilla method. The display name is configurable (`keyDisplayName`).

## Muzzle

**Right-click** a cuffed player while holding **black wool** to muzzle them, consuming the wool.

- A muzzled player's microphone is silenced via the Simple Voice Chat mod integration.
- Muzzle state is tracked alongside cuff state and is removed when cuffs are removed.

Requires [Simple Voice Chat](https://modrinth.com/plugin/simple-voice-chat) to be installed on the server. Without it, the black wool interaction has no effect.

## Degradation

Cuffs lose durability over **real-world time**, including while the player is offline.

- Default rate: 0.05 durability/second → cuffs last roughly **33 minutes** from full durability.
- When durability reaches zero, cuffs break automatically, restoring the prisoner's inventory.
- On login, elapsed offline time is applied immediately; cuffs that would have broken while offline break the moment the player connects.

## Repairing Cuffs

**Right-click** a cuffed player while holding an **iron ingot** to repair their cuffs.

- Each ingot restores 25 durability (configurable).
- Ingots are not consumed if the cuffs are already at maximum durability.

## Death

If a cuffed player dies:

- Their stored (hidden) inventory is dropped at the death location.
- Cuffs, muzzle, and any chain are removed.
- The player respawns normally.

## Persistence

Cuff state survives server restarts. The state is saved to `world/ivanica_cuffs.json` and inventories are stored in `world/ivanica_inventories/<uuid>.nbt`.

## Configuration

| Key | Default | Effect |
|---|---|---|
| `handcuffMaxDurability` | `100` | Starting and maximum durability |
| `handcuffDegradePerSecond` | `0.05` | Durability lost per real-time second |
| `handcuffRepairPerIngot` | `25` | Durability restored per iron ingot |
| `handcuffItemId` | `"minecraft:chain"` | Base item for the handcuff item |
| `handcuffDisplayName` | `"Handcuffs"` | Display name on the handcuff item |
| `handcuffRecipeIngredients` | 3× iron ingot + 2× string | Shapeless crafting ingredients |
| `keyItemId` | `"minecraft:tripwire_hook"` | Base item for the key |
| `keyDisplayName` | `"Handcuff Key"` | Display name on the key item |

See [Configuration](../configuration.md) for the full reference.
