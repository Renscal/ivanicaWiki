# Chains

A chain lets you tether a cuffed player to yourself and drag them around — useful for escorting prisoners.

## Attaching a Chain

1. Hold a **`minecraft:iron_chain`** item.
2. **Shift + right-click** a cuffed player.
3. The chain item is consumed and the prisoner is tethered to you.

A player can only be chained once — attempting to chain an already-chained player sends an error.

## Visual

A line of **gray particles** connects you and the prisoner, visible to all nearby players. The particles are emitted every 2 ticks between the bottom of each player's head.

## Behaviour

- If the prisoner moves beyond their current max distance from you, the server teleports them back each tick (rubber-band).
- The prisoner's normal restrictions (no sprint, empty inventory, etc.) from the [handcuffs](handcuffs.md) still apply.

## Adjusting Chain Length

While the chain is active, you can adjust the max tether distance by **sneaking and scrolling**:

- **Scroll up** — extend the chain (up to 10 blocks)
- **Scroll down** — pull the chain shorter (minimum 1 block)
- Step size: 0.5 blocks per scroll click
- Default distance: 5 blocks

A chat message confirms the new distance each time you adjust.

## Removing a Chain

A chain is removed only when the cuffs come off:

| Event | Result |
|---|---|
| Key used on the prisoner | Cuffs and chain removed |
| Cuffs degrade to zero | Cuffs and chain removed |
| Prisoner dies | Cuffs and chain removed, stored inventory dropped |

There is no way to remove the chain while keeping the prisoner cuffed.

## Logout Behaviour

If the chained prisoner **logs out**, the chain is preserved. Their [sleeping body](sleeping.md) spawns as normal and the chainer can still drag it — the body rubber-bands to the chainer just like a live player. When the prisoner reconnects, they appear at wherever their body ended up.

If the **chainer** logs out while holding a chain, the particle visual disappears (their entity is gone) but the chain state is preserved. On reconnect, the particle visual resumes if the prisoner or their body is still present.

The chain is in-memory only — a **server restart** removes all active chains. Prisoners are still cuffed after a restart but must be re-chained manually.
