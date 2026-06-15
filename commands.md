# Commands

## /surrender

**Usage:** `/surrender`  
**Available to:** All players  
**Permission:** None

Puts the player into the [downed state](mechanics/down-mode.md), as if they had taken fatal damage. Does nothing if the player is already downed or is currently cuffed.

---

## /nation

All nation commands are under the `/nation` prefix.

### Player commands

| Command | Who | Description |
|---|---|---|
| `/nation create <name>` | Anyone (no nation) | Create a new nation and become its leader |
| `/nation invite <player>` | Leader | Send an invite to a player |
| `/nation accept <nation>` | Invited player | Accept a pending invite |
| `/nation kick <player>` | Leader | Remove a member; their chunk slot contribution is held for 7 days |
| `/nation leave` | Members (not leader) | Leave your nation; your slot contribution is held for 7 days |
| `/nation info [nation]` | Anyone | Show info about your own nation or a named one |
| `/nation list` | Anyone | List all nations |
| `/nation claim` | Leader | Claim the chunk you are standing in |
| `/nation unclaim` | Leader | Release the chunk you are standing in |
| `/nation chunks` | Any member | Show used / pool / remaining chunk slots |
| `/nation color <hex>` | Leader | Set nation color (6-digit hex, e.g. `FF4400`) |
| `/nation showclaim` | Any member | Toggle green particle visualization of your nation's claim borders |

### Staff commands

These require **GAMEMASTERS** permission level.

| Command | Description |
|---|---|
| `/nation rename <nation> <newname>` | Rename a nation |
| `/nation delete <nation>` | Begin a delete — sets a pending confirmation |
| `/nation confirm` | Confirm and execute the pending delete |

Deleting a nation removes its scoreboard team, unclaims all its chunks, and clears all member associations.
