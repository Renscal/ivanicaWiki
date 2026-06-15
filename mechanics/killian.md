# Killian

Killian is a unique cursed netherite sword. Picking it up by killing with it permanently binds the wielder to it — granting power while it is in their inventory and punishing them when it is not.

## Obtaining Killian

Killian cannot be crafted or found in loot. It is given exclusively by staff:

```
/ivanica give killian <player> [count]
```

Requires **GAMEMASTERS** permission.

## Becoming Bound

Any player who **kills another player** while holding Killian in their **main hand** becomes bound to it. Binding is permanent until removed by a staff member.

- Binding persists across server restarts (`world/ivanica_killian_bound.json`).
- Multiple players can be bound simultaneously.
- Staff can remove a binding with `/ivanica unbind <player>`.

## Effects While Bound

### Killian In Inventory

While Killian is anywhere in the bound player's inventory:

| Effect | Details |
|---|---|
| +20 max HP | One extra row of hearts (permanent attribute modifier) |
| Glowing | Applied every tick — always visible through walls |
| Regeneration I | Applied every tick while Killian is in the **main hand** |
| Killian Armor | Full netherite set is auto-equipped on binding and re-equipped if any piece goes missing |

### Killian Not In Inventory

While Killian is **not** in the bound player's inventory:

| Effect | Details |
|---|---|
| Weakness | Applied for 5 hours; refreshed whenever it drops below 100 ticks |
| Hunger | Applied for 5 hours; refreshed whenever it drops below 100 ticks |
| Wither I | Applied continuously while food level is below 2 |
| Armor stripped | All Killian armor pieces are removed immediately |
| Health modifier removed | Max HP returns to normal |
| Auto-unbind | After **5 consecutive real-world hours** without Killian, the binding is automatically lifted |

The 5-hour timer is wall-clock time (not server ticks). It resets whenever Killian re-enters the inventory. The timer persists across server restarts in `world/ivanica_killian_bound.json`.

## Killian Armor

On becoming bound, the player is immediately equipped with a full netherite armor set:

- Each piece has **Curse of Binding** and **Curse of Vanishing** (always applied).
- Additional enchantments are configurable per piece in `config/ivanica/killian.json`.
- The armor is identified by `ivanica_killian_armor` in its item NBT.
- While Killian is in inventory, missing armor pieces are automatically re-equipped each tick.
- The armor is stripped on unbind or whenever Killian leaves the inventory.

## Special Abilities

### Water Walking

While Killian is in any hotbar slot (0–8), the bound player **walks on water**. The server sends the client a fake solid block at the topmost water block in the player's column, making the client treat the water surface as solid ground. Server-side downward velocity is zeroed each tick to keep the entity in sync.

- **Sneaking** cancels water walking for that tick.

### Telekinesis

While holding Killian, **right-click** a player or hostile mob to grab them with telekinesis.

- The target is held `reachBlocks` blocks in front of the holder and repositioned every tick.
- **Exhaustion** is drained each tick (from saturation first at 2× rate, then food level).
- If the target's path is blocked by a block, they are **slammed** into it (`slamDamage`) and released. A 10-second cooldown then applies before telekinesis can be activated again.
- Releasing without a slam carries the target's momentum forward.
- **Sneaking while holding** deals `ATTACK_DAMAGE` every 10 ticks, doubles exhaustion drain, and turns the particle beam fully red.
- Telekinesis ends automatically if Killian leaves the main hand or the holder's food level reaches 0.

Telekinesis activates either through direct right-click (within normal reach) or via a server-side raycast up to `reachBlocks` distance. Targets: players and hostile mobs. **Excluded:** Wither.

### Offhand Restriction

While Killian is in any hotbar slot, the bound player **cannot hold anything in the offhand**. Any item found there is dropped to the ground each tick. Right-clicking with the offhand (against blocks, entities, or in the air) is also blocked.

## Dropping Killian

Pressing **Q** to drop Killian does **not** create an item entity. Instead:

- An **invisible, invulnerable armor stand** is spawned at the drop location holding Killian.
- **Right-click the stand** to retrieve Killian (it is placed into your inventory and the stand despawns).
- **No player** can place Killian into item frames or equip it to armor stands (applies regardless of binding).
- If [Dynmap](../discord.md) is installed, a skull marker appears on the web map at the stand's location and is removed when the sword is picked up.

## Inventory Lock

**No player** can use the GUI to move Killian into an external inventory (chest, furnace, etc.), regardless of binding. Shift-clicking and hotbar-swap into external slots are also blocked. Q-drop is the only way to remove Killian from a player inventory.

## Unbinding

Unbinding removes all Killian effects:

- Binding entry removed from `boundPlayers`.
- Killian armor stripped.
- +20 HP modifier removed.
- Weakness and Hunger cleared.
- 5-hour timer reset.

**Staff command:** `/ivanica unbind <player>` (GAMEMASTERS)

## Configuration

### `config/ivanica/killian.json`

Enchantment lists per armor piece. Each entry is `{ "enchantment": "<id>", "level": <n> }`.

| Key | Default enchantments |
|---|---|
| `helmetEnchantments` | Protection IV, Unbreaking III, Respiration III, Aqua Affinity I |
| `chestplateEnchantments` | Protection IV, Unbreaking III |
| `leggingsEnchantments` | Protection IV, Unbreaking III |
| `bootsEnchantments` | Protection IV, Unbreaking III, Feather Falling IV |

Curse of Binding and Curse of Vanishing are always applied regardless of this list.

### `config/ivanica/telekinesis.json`

| Key | Default | Effect |
|---|---|---|
| `reachBlocks` | `8.0` | Distance in front of the holder where the target is held |
| `exhaustionPerTick` | `0.1` | Exhaustion per tick (doubled while sneaking) |
| `slamDamage` | `12.0` | Damage dealt when the target hits a block |

See [Configuration](../configuration.md) for the full reference.
