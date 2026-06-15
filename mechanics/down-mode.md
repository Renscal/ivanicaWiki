# Down Mode

When a player would normally die they are instead put into a **downed** state.

## Flow

```
player takes fatal damage
        │
        ▼
   DOWNED STATE
   • crawl pose (SWIMMING)
   • health locked at 0.5 HP
   • Slowness II applied
   • cannot jump
   • action bar shows time remaining
        │
        ├─── invincibility window (default 10 s)
        │    all incoming damage is blocked
        │         │
        │         ▼
        │    ONE-SHOT WINDOW
        │    any hit deals a real, lethal kill
        │
        ├─── bleed-out timer (default 60 s)
        │    if timer expires, player dies automatically
        │
        └─── revival
             non-downed player crouches within 2 blocks
             for the revive duration (default 5 s)
             → player is revived at configurable HP
```

## Being Downed

- The downed player cannot die from normal damage during the invincibility window.
- After invincibility expires, the next hit from any source kills them for real.
- Slowness II is continuously re-applied each tick so it cannot be removed by other effects.
- The action bar shows remaining bleed-out time and the current phase (`INVINCIBLE` / `ONE-SHOT`).
- Down state is not persisted — a server restart or player disconnect clears it.

## Revival

A teammate can revive a downed player by:

1. Standing within **2 blocks** of the downed player.
2. Holding **crouch (sneak)** continuously for the revive duration (default 5 seconds).
3. Progress resets if the reviver moves out of range or stops crouching.
4. On completion the downed player stands up, receives configurable health, and all down-related effects are removed.

Only one reviver can be active at a time. If a different player steps in, progress resets from zero.

## Surrendering

Any player can use `/surrender` to put themselves into the downed state voluntarily. The command is blocked while the player is cuffed. See [Commands](../commands.md).

## Configuration

| Key | Default | Effect |
|---|---|---|
| `downedDurationSeconds` | `60` | Seconds until bleed-out kills the player |
| `invincibilityDurationSeconds` | `10` | Seconds of invincibility after being downed |
| `reviveDurationSeconds` | `5` | Seconds a reviver must hold crouch |
| `reviveHealthAmount` | `4.0` | HP restored on revive (1 HP = 0.5 hearts) |

See [Configuration](../configuration.md) for the full reference.
