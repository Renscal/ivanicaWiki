# Nations

Nations let players group together, claim territory, and protect it from outsiders.

## Creating a Nation

```
/nation create <name>
```

You become the leader. A nation has one leader and any number of members.

## Members

| Action | Command | Who |
|---|---|---|
| Invite a player | `/nation invite <player>` | Leader |
| Accept an invite | `/nation accept <nation>` | Invited player |
| Kick a member | `/nation kick <player>` | Leader |
| Leave the nation | `/nation leave` | Members (not leader) |

Leaders cannot leave — the nation must be deleted instead (staff command).

## Claiming Chunks

The leader claims and unclaims chunks with:

```
/nation claim      — claim the chunk you are standing in
/nation unclaim    — release the chunk you are standing in
/nation chunks     — show used / pool / remaining
```

**First claim:** Any unclaimed chunk becomes the nation's **core**.

**Subsequent claims:** Every new claim must be **chunk-adjacent** (sharing a border with a chunk already owned by the nation). You cannot claim isolated chunks.

### Chunk Pool

Each member contributes `claimChunksPerMember` slots (default 5) to the pool:

```
pool = members × claimChunksPerMember
```

You cannot claim beyond the pool.

### Slot Cooldown

When a member **leaves or is kicked**, their slot contribution is not immediately removed. The nation keeps those slots for **7 days**. After 7 days, if the nation has claimed more chunks than the new pool allows, the **newest excess claims** are automatically removed and the leader receives a warning.

If the departing player joins another nation before their cooldown expires, that new nation does **not** gain the slots until the cooldown ends.

Rejoining the original nation cancels the cooldown immediately.

## Chunk Protection

In a claimed chunk, **non-members** are blocked from:

- Breaking blocks *(exception: beds can always be broken)*
- Placing blocks
- Interacting with blocks (doors, buttons, levers, etc.)

**Inventory blocks** (chests, barrels, furnaces, etc.) are always accessible to everyone.

**Explosions** bypass the protection entirely — they are not blocked.

## Info and Display

```
/nation info [name]   — show nation info (defaults to your own)
/nation list          — list all nations
/nation color <hex>   — set name color (6-digit hex, e.g. FF4400)
/nation showclaim     — toggle green particle visualization of your nation's claim borders
```

Nation color is applied as a scoreboard team prefix and controls nametag glow color.

## Persistence

Nation state is saved to `world/ivanica_nations.json` and survives server restarts.

## Configuration

| Key | Default | Effect |
|---|---|---|
| `claimChunksPerMember` | `5` | Chunk slots per member |

See [Configuration](../configuration.md) for the full reference.
