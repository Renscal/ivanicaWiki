# Sleeping (Logout Bodies)

When a player disconnects, a **sleeping body** is left behind at their location. Other players can see it, interact with it, and kill it.

## The Sleeping Body

- Spawns immediately when the player disconnects.
- Uses the player's real game profile — skin and name are identical.
- Appears in a **sleeping pose** (lying on the ground, like a bed but without one).
- Cannot move or be moved.
- Persists across **server restarts** — the body is re-spawned at the saved location on startup.

## Damage Rules

Only **direct player attacks** can damage the sleeping body. All other damage (fire, lava, explosions, fall damage, etc.) is silently ignored.

## Killing the Body

If the sleeping body is killed:

1. The player's **inventory snapshot** (taken at the moment they logged out) is dropped at the body's location.
2. The player is flagged as **killed offline**.
3. On their **next login**, they are immediately killed — they spawn, their inventory is cleared, and they die. This awards a kill as if they had been killed normally.

If the body was killed while the player was **cuffed**, the stored (hidden) inventory is also dropped at the body's location — not the snapshot, which was empty at that point.

## Normal Reconnect

If the player reconnects while the body is still alive:

1. The body is despawned.
2. The inventory snapshot is deleted.
3. The player resumes normally with their inventory intact.

## Cuff and Chain Compatibility

**Cuff state is fully preserved** through a logout — a cuffed player is still cuffed when they reconnect.

**Chain state is also preserved.** If the prisoner was chained when they logged out, their sleeping body can still be dragged by the chainer. When the prisoner reconnects they appear at the body's current location — wherever it was dragged to.

The chain is in-memory only and is cleared on a **server restart**. The prisoner will still be cuffed but must be re-chained manually after a restart.

See [Handcuffs](handcuffs.md) and [Chains](chains.md) for more detail.
