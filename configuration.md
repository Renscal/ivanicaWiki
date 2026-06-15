# Configuration Reference

Ivanica uses seven separate JSON config files, one per feature. They live in your server's `config/` directory and are created with defaults on first launch. A server restart is required to apply changes.

---

## Messages system

Every config file that has player-facing text contains a `messages` object. All strings support Minecraft formatting codes (`§a`, `§c`, etc.) and `{placeholder}` substitution. Placeholders are replaced at the point the message is sent — the available placeholders are listed per-config below.

**Example** — changing the downed announcement:
```json
"messages": {
  "downedBroadcast": "§4{player} §chas been downed! Help them!"
}
```

Placeholders not listed for a given message are silently left as-is (they won't cause errors, but they won't be replaced either).

---

## `config/ivanica/down.json`

Controls the down-mode (downed state, revive, bleed-out).

### Settings

| Key | Default | Description |
|-----|---------|-------------|
| `downedDurationSeconds` | `60` | How long a downed player has before they bleed out and die. |
| `reviveDurationSeconds` | `5` | How long a reviver must hold sneak to complete a revive. |
| `reviveHealthAmount` | `4.0` | HP restored on revive (in half-hearts; 4.0 = 2 full hearts). |
| `invincibilityDurationSeconds` | `10` | Window after being downed during which the player cannot be hit at all. |
| `downedHealthAmount` | `0.5` | Health the player is set to when they are downed (in half-hearts; 0.5 = ¼ heart). Must be above 0. |
| `reviveRadiusBlocks` | `2.0` | Maximum distance in blocks between a crouching reviver and the downed player. |
| `downedSlownessAmplifier` | `1` | Slowness effect level while downed. `0` = Slowness I, `1` = Slowness II, `-1` = no slowness. |
| `surrenderEnabled` | `true` | If `false`, the `/surrender` command is not registered at all. |

### Messages

Placeholders: `{player}` player name, `{reviver}` reviver name, `{target}` downed player name, `{secs}` seconds remaining, `{phase}` current phase label, `{pct}` revive progress 0–100.

| Key | Default | Notes |
|-----|---------|-------|
| `downed` | `§cYou are downed! A teammate can revive you by crouching near you.` | Shown to the downed player. |
| `downedBroadcast` | `{player} has been downed!` | Broadcast to all players. |
| `oneShotWarning` | `§cYou are now vulnerable — one hit will finish you!` | Shown to the downed player when the invincibility window ends. |
| `bledOut` | `{player} bled out.` | Broadcast when bleed-out timer expires. |
| `reconnectedDowned` | `§cYou are still downed from before the server restarted!` | Shown to a downed player on login (down state does not survive restarts, so this is unused by default). |
| `actionBar` | `§cDOWNED§r — bleeding out in §e{secs}s §r[{phase}§r]` | Shown on the downed player's action bar every tick. Uses `{phase}` resolved to `phaseInvincible` or `phaseOneShot`. |
| `phaseInvincible` | `§aINVINCIBLE` | Phase label shown in action bar during invincibility window. |
| `phaseOneShot` | `§cONE-SHOT` | Phase label shown in action bar when one hit can finish them. |
| `revivedSelf` | `§aYou were revived by §e{reviver}§a!` | Shown to the player who was revived. |
| `revivedActor` | `§aYou revived §e{player}§a!` | Shown to the reviver. |
| `revivedBroadcast` | `{reviver} revived {player}!` | Broadcast to all players. |
| `revivingProgress` | `Reviving §e{target}§r... §a{pct}%` | Shown on the reviver's action bar while crouching. |
| `surrenderOnlyPlayers` | `Only players can surrender.` | Error shown when a command block/console runs `/surrender`. |
| `surrenderCuffed` | `You cannot surrender while cuffed.` | Error shown when a cuffed player tries to surrender. |
| `surrenderAlreadyDowned` | `You are already downed.` | Error shown when a downed player runs `/surrender` again. |

---

## `config/ivanica/cuff.json`

Controls handcuffs, keys, bag, chain.

### Settings

| Key | Default | Description |
|-----|---------|-------------|
| `handcuffMaxDurability` | `100.0` | Starting and maximum durability for new cuffs. |
| `handcuffDegradePerSecond` | `0.05` | Durability lost per real-world second (applies while the cuffed player is online or offline). At the default, cuffs last ~33 minutes total. |
| `handcuffRepairPerIngot` | `25.0` | Durability restored when a cuffed player is right-clicked with an iron ingot. |
| `handcuffItemId` | `minecraft:chain` | The base Minecraft item that acts as the handcuff custom item. Must be a valid item ID. |
| `handcuffDisplayName` | `Handcuffs` | Display name on the handcuff item. Supports `§` colour codes. |
| `handcuffRecipeIngredients` | `[iron_ingot ×3, string ×2]` | Shapeless crafting recipe as a JSON array of item ID strings. Duplicates are allowed (they count as separate slots). |
| `keyItemId` | `minecraft:tripwire_hook` | Base Minecraft item for the key. |
| `keyDisplayName` | `Handcuff Key` | Display name on the key item. |
| `bagBlindnessAmplifier` | `0` | Blindness level applied to a bagged player. `0` = Blindness I, `1` = Blindness II, `-1` = no blindness. |
| `bagBlindnessDurationTicks` | `60` | How long the Blindness effect lasts (in ticks; 20 ticks = 1 second). |
| `bagBlindnessRefreshThresholdTicks` | `40` | Blindness is re-applied when the remaining duration falls below this value. Lower values = more frequent top-ups. |
| `observerRadiusBlocks` | `16.0` | Distance within which other players can see a cuffed player's durability bar. |
| `observerGazeDegrees` | `14.0` | Half-angle of the gaze cone (degrees). An observer outside this cone of their look direction won't see the durability bar. |
| `chainDefaultDistance` | `5.0` | Default max chain length (blocks) when a chain is first applied. |
| `chainMinDistance` | `1.0` | Minimum chain length the chainer can scroll down to. |
| `chainMaxDistance` | `10.0` | Maximum chain length the chainer can scroll up to. |
| `chainDistanceStep` | `0.5` | How much chain length changes per shift-scroll tick. |

### Messages

Placeholders: `{player}` cuffed player name, `{actor}` actor name, `{added}` durability added, `{current}` current durability, `{max}` max durability, `{durability}` current durability, `{count}` item count, `{length}` chain distance, `{bar}` coloured block progress bar, `{color}` colour code (`§a`/`§e`/`§c`), `{pct}` integer percentage.

| Key | Default | Notes |
|-----|---------|-------|
| `handcuffedTarget` | `§8You have been handcuffed.` | Shown to the cuffed player. |
| `handcuffedActor` | `§7You handcuffed §e{player}§7.` | Shown to the player who applied cuffs. |
| `cuffsFullDurability` | `§7The handcuffs are already at full durability.` | Error when trying to repair already-full cuffs. |
| `cuffRepaired` | `§7Repaired handcuffs (+§a{added}§7). Now §a{current}§7/§a{max}§7.` | Shown to the actor after repairing. |
| `wrongKey` | `§cThis is not the right key.` | Error when using a key on the wrong player. |
| `uncuffedTarget` | `§aYour handcuffs were removed by §e{actor}§a.` | Shown to the uncuffed player. |
| `uncuffedActor` | `§7You uncuffed §e{player}§7.` | Shown to the player who used the key. |
| `cuffsBroke` | `§aYour handcuffs broke!` | Shown to the freed player when durability hits zero. |
| `cuffsBrokeBroadcast` | `{player}'s handcuffs broke!` | Broadcast to all players. |
| `storedItemsDropped` | `{player}'s stored items were dropped.` | Broadcast when cuffed inventory is dropped (death or NPC kill). |
| `storedItemsReturned` | `§aYour stored items have been returned — your handcuffs were removed while you were offline.` | Shown on login if cuffs broke offline. |
| `cuffsOfflineBroke` | `§aYour handcuffs broke while you were offline!` | Shown on login if degradation finished while offline. |
| `cuffsLoginReminder` | `§8You are still handcuffed. Durability: §c{durability}§8/§c{max}§8.` | Shown on login if still cuffed. |
| `alreadyBagged` | `§cThis player already has a bag on.` | Error when trying to bag an already-bagged player. |
| `baggedTarget` | `§8A bag has been put on your head.` | Shown to the bagged player. |
| `baggedActor` | `§7You bagged §e{player}§7.` | Shown to the actor. |
| `bagRemovedActor` | `§7You removed the bag from §e{player}§7.` | Shown to the actor on bag removal. |
| `bagRemovedTarget` | `§7Your bag has been removed.` | Shown to the unbagged player. |
| `alreadyChained` | `§cThis player is already chained.` | Error when trying to chain an already-chained player. |
| `chainedTarget` | `§8You have been chained to §e{actor}§8.` | Shown to the chained player. |
| `chainedActor` | `§7You chained §e{player}§7 to yourself.` | Shown to the chainer. |
| `chainRemovedActor` | `§7You removed the chain from §e{player}§7.` | Shown to the chainer on removal. |
| `chainRemovedTarget` | `§7Your chain has been removed.` | Shown to the unchained player. |
| `chainLength` | `§7Chain length: §e{length}m` | Action bar message shown to the chainer after adjusting length. |
| `chainExtended` | `§7Your chain has been §7extended§7.` | Feedback to chainer on scroll-up. |
| `chainPulled` | `§7Your chain has been §cpulled in§7.` | Feedback to chainer on scroll-down. |
| `chainAnchored` | `§7You anchored the chain to a post.` | Shown to the chainer when they anchor to a fence/wall. |
| `chainAnchoredTarget` | `§8Your chain has been anchored to a post.` | Shown to the chained player when anchored. |
| `chainAnchorBroke` | `§7Your chain broke free from the post.` | Shown to the chained player when the anchor block is broken. |
| `cuffBarSelf` | `§8Cuffs: {bar} {color}{pct}%` | Action bar shown to the cuffed player (when down-mode is not already using the bar). |
| `cuffBarObserver` | `§e{player} §8cuffs: {bar} {color}{pct}%` | Action bar shown to nearby observers looking at the cuffed player. |
| `gaveHandcuffs` | `Gave {count} handcuff(s) to {player}` | Feedback message for `/ivanica give handcuffs`. |

---

## `config/ivanica/logout.json`

Controls the logout NPC mechanic.

### Settings

| Key | Default | Description |
|-----|---------|-------------|
| `enabled` | `true` | Master toggle. Set `false` to disable the logout NPC entirely — players who disconnect simply vanish with no NPC body. |
| `protectionSeconds` | `30` | How many seconds after logging out the NPC body cannot be attacked. Set `0` to allow immediate attack. |
| `showInTabList` | `true` | Whether the NPC appears in the player list tab while active. |

### Messages

Placeholders: `{seconds}` protection duration in seconds.

| Key | Default | Notes |
|-----|---------|-------|
| `logoutProtected` | `§cThis player is protected for {seconds}s after logging out.` | Shown to a player who attacks a protected NPC body. |
| `killedOffline` | `§cYou were killed while offline!` | Shown on login to a player whose NPC body was killed. |

---

## `config/ivanica/muzzle.json`

Controls the voice-chat muzzle (requires Simple Voice Chat mod).

### Settings

| Key | Default | Description |
|-----|---------|-------------|
| `muteOnBag` | `true` | If `true`, microphone packets from bagged players are cancelled so other players cannot hear them through Simple Voice Chat. Has no effect if Simple Voice Chat is not installed. |

---

## `config/ivanica/nation.json`

Controls the nations and chunk-claiming system.

### Settings

| Key | Default | Description |
|-----|---------|-------------|
| `claimChunksPerMember` | `5` | Number of chunk slots each nation member contributes to the pool. A 10-member nation can claim up to 50 chunks. |
| `protectBeds` | `false` | If `true`, outsiders cannot break beds in claimed chunks. If `false`, beds are always breakable. |
| `departureCooldownDays` | `7` | When a member leaves or is kicked, their slot contribution is locked for this many days before the nation's pool shrinks. If the pool would drop below current claim count, the newest excess claims are automatically removed. |
| `showClaimRadiusChunks` | `5` | How many chunks outward from the player's position `/nation showclaim` renders border particles. |
| `friendlyFireDefault` | `false` | Whether friendly fire is enabled for newly created nations. |
| `protectInventoryBlocks` | `false` | If `true`, outsiders cannot open chests, furnaces, or any other inventory block in claimed chunks. If `false`, inventory blocks are always accessible (the default). |
| `tntBypassesClaims` | `true` | If `true`, any player can place/throw TNT in claimed chunks regardless of ownership. If `false`, TNT placement is subject to the same protection rules as other blocks. |

### Messages

Placeholders: `{nation}` nation name, `{player}` player name, `{leader}` leader name, `{days}` departure cooldown days, `{count}` chunk count, `{used}` chunks claimed, `{pool}` total chunk pool, `{remaining}` chunks remaining, `{old}` old nation name, `{new}` new nation name, `{name}` nation name, `{state}` friendly-fire state label.

| Key | Default | Notes |
|-----|---------|-------|
| `chunkProtected` | `§cThis chunk is claimed by §e{nation}§c.` | Shown to an outsider who tries to break/interact in a claimed chunk. |
| `kickedFromNation` | `§cYou were kicked from §e{nation}§c.` | Shown to the kicked player. |
| `memberLeft` | `§e{player} §7left your nation. Their chunk slots are held for §e{days} days§7.` | Shown to the nation leader. |
| `chunkSlotsExpired` | `§cA departed member's chunk slots expired. §e{count} chunk(s) were automatically unclaimed.` | Shown to the leader when auto-unclaim triggers. |
| `errNationExists` | `§cA nation with that name already exists, or that player is already in a nation.` | Error on `/nation create`. |
| `nationCreated` | `§aCreated nation §e{nation}§a with leader §e{leader}§a.` | Shown to the staff member who created the nation. |
| `nationLeader` | `§aYou are now the leader of nation §e{nation}§a.` | Shown to the new leader. |
| `errInvalidColor` | `§cInvalid color. Provide a 6-digit hex code, e.g. §rFF5500§c or §r#FF5500§c.` | Error on `/nation color`. |
| `errNotLeader` | `§cYou are not a nation leader.` | Generic error for leader-only commands. |
| `errInviteSelf` | `§cYou cannot invite yourself.` | Error on `/nation invite`. |
| `errInviteFailed` | `§cCould not invite that player — they may already be in a nation.` | Error when invite cannot be sent. |
| `inviteSent` | `§7Invited §e{player} §7to §e{nation}§7.` | Shown to the inviting player. |
| `inviteReceived` | `§7You have been invited to nation §e{nation}§7. Type §a/nation accept {nation} §7to join.` | Shown to the invited player. Note: `{nation}` appears twice. |
| `errAlreadyInNation` | `§cYou are already in a nation.` | Error on `/nation accept`. |
| `errNoInvite` | `§cNo pending invite from that nation, or the nation no longer exists.` | Error when trying to accept a non-existent invite. |
| `joinedNation` | `§aYou have joined nation §e{nation}§a.` | Shown to the joining player. |
| `memberJoined` | `§e{player} §7has joined your nation.` | Shown to the existing nation members. |
| `errPlayerNotFound` | `§cPlayer '§r{player}§c' not found.` | Error when the target player does not exist on this server. |
| `errKickFailed` | `§cCould not kick that player — they may not be in your nation.` | Error on `/nation kick`. |
| `kicked` | `§7Kicked §e{player} §7from §e{nation}§7. Their chunk slots are held for §e{days} days§7.` | Shown to the leader who performed the kick. |
| `errNotInNation` | `§cYou are not in a nation.` | Error when a nation command requires the player to be in a nation. |
| `errLeaderCannotLeave` | `§cLeaders cannot leave — use §r/nation delete §cto disband the nation.` | Error on `/nation leave` for leaders. |
| `leftNation` | `§7You left §e{nation}§7.` | Shown to the player who left. |
| `errAlreadyClaimed` | `§cYour nation already claims this chunk.` | Error on `/nation claim`. |
| `errNotAdjacent` | `§cNew claims must be adjacent to an existing claim.` | Error when a claim is not contiguous. |
| `errNoSlots` | `§cNo claim slots remaining. §7({used}/{pool} used)` | Error when the claim pool is full. |
| `chunkClaimed` | `§aChunk claimed. §7({used}/{pool} slots used)` | Confirmation on successful claim. |
| `errNotOwned` | `§cYour nation does not claim this chunk.` | Error on `/nation unclaim`. |
| `chunkUnclaimed` | `§7Chunk unclaimed. §7({used}/{pool} slots used)` | Confirmation on unclaim. |
| `errNationNotFound` | `§cNation '§r{name}§c' not found.` | Error for staff commands targeting a non-existent nation. |
| `errRenameConflict` | `§cNation '§r{old}§c' not found, or '§r{new}§c' is already taken.` | Error on `/nation rename`. |
| `renamed` | `§aRenamed nation §e{old} §ato §e{new}§a.` | Shown to the staff member. |
| `deleteConfirm` | `§eType §a/nation confirm §ato permanently delete nation §e{nation}§e. This cannot be undone.` | Prompt shown before deletion. |
| `errNoPendingDelete` | `§cNo pending deletion. Use §r/nation delete <nation> §cfirst.` | Error on `/nation confirm` with no pending delete. |
| `errNationGone` | `§cNation '§r{name}§c' no longer exists.` | Error when the nation was deleted between `/nation delete` and `/nation confirm`. |
| `deleted` | `§cDeleted nation §e{name}§c.` | Confirmation on deletion. |
| `errNotFfLeader` | `§cOnly the nation leader can toggle friendly fire.` | Error on `/nation friendlyfire`. |
| `ffToggleSelf` | `§7Friendly fire for §e{nation} §7is now {state}§7.` | Shown to the leader. `{state}` resolves to `ffStateOn` or `ffStateOff`. |
| `ffToggleMember` | `§7Friendly fire is now {state} §7in §e{nation}§7.` | Shown to all members. |
| `ffStateOn` | `§aON` | Label used in friendly-fire messages. |
| `ffStateOff` | `§cOFF` | Label used in friendly-fire messages. |
| `showclaimOn` | `§aChunk visualization §l§aON§r§a. Green particles mark your nation's borders.` | Shown when enabling claim visualization. |
| `showclaimOff` | `§7Chunk visualization §l§7OFF§r§7.` | Shown when disabling claim visualization. |
| `chunksInfo` | `§e{nation} §7chunks: §a{used}§7/§a{pool} §7claimed, §a{remaining} §7remaining` | Output of `/nation chunks`. |

---

## `config/ivanica/tnt.json`

Controls the TNT-throw mechanic.

### Settings

| Key | Default | Description |
|-----|---------|-------------|
| `fuseTicks` | `80` | Fuse length of spawned TNT in game ticks (20 ticks = 1 second; default 80 = 4 seconds). |
| `power` | `4.0` | Explosion power. Vanilla TNT is `4.0`. Higher values create larger, more destructive explosions. The power is applied via `world.createExplosion` one tick before the entity would naturally explode, bypassing the hardcoded vanilla value. |

---

## `config/ivanica/discord.json`

Controls Discord integration. See [DISCORD.md](DISCORD.md) for setup instructions.

### Settings

| Key | Default | Description |
|-----|---------|-------------|
| `enabled` | `false` | Master toggle. Must be `true` along with valid `botToken` and `guildId` for any Discord features to activate. |
| `botToken` | `""` | Bot token from the Discord Developer Portal. Supply the raw token (do not include the `Bot ` prefix). |
| `guildId` | `""` | Numeric ID of the Discord server where nation roles and the `/link` slash command will be managed. |

---

## Minecraft colour codes

All message strings support the standard Minecraft formatting codes using the `§` prefix:

| Code | Colour/Style |
|------|-------------|
| `§0` | Black |
| `§1` | Dark Blue |
| `§2` | Dark Green |
| `§3` | Dark Aqua |
| `§4` | Dark Red |
| `§5` | Dark Purple |
| `§6` | Gold |
| `§7` | Gray |
| `§8` | Dark Gray |
| `§9` | Blue |
| `§a` | Green |
| `§b` | Aqua |
| `§c` | Red |
| `§d` | Light Purple |
| `§e` | Yellow |
| `§f` | White |
| `§l` | **Bold** |
| `§o` | *Italic* |
| `§n` | Underline |
| `§m` | Strikethrough |
| `§k` | Obfuscated (random characters) |
| `§r` | Reset formatting |

To clear all formatting and return to default white text, end with `§r`.
