# TNT Throw

Right-clicking with a `minecraft:tnt` item on a block face throws a **primed TNT** instead of placing the block.

## Usage

1. Hold **TNT** in your hand.
2. **Right-click** on a block surface.
3. The TNT item is consumed and a primed TNT entity spawns at the placement position — the block face you clicked.

The TNT will count down and explode after the configured fuse time.

## Sticky Mode

**Shift + right-click** to throw a **sticky** TNT:

- The entity has no gravity — it stays exactly where it lands.
- Useful for attaching TNT to vertical walls or ceilings.

Without shift, the TNT falls normally under gravity.

## Claimed Chunks

TNT can be thrown **anywhere**, including into chunks claimed by other nations. It is intentionally exempt from claim protection — it functions as a siege weapon.

## Configuration

| Key | Default | Effect |
|---|---|---|
| `tntFuseTicks` | `80` | Ticks before the TNT explodes (20 ticks = 1 s; default = 4 s) |
| `tntPower` | `4.0` | Explosion power (vanilla TNT = 4.0; higher = larger blast) |

See [Configuration](../configuration.md) for the full reference.
