# Discord Integration

Ivanica can mirror your server's nation structure onto a Discord guild: when a nation is created, two Discord roles are created automatically (`NationName` and `NationName Leader`). As players join, leave, or are kicked from nations, their Discord roles are updated in real time. Players link their Minecraft and Discord accounts with a short code exchange.

---

## Requirements

- A Discord bot with **Manage Roles** and **Use Slash Commands** (Applications.Commands) permissions
- The bot's role in Discord must be **above** every nation role in the role hierarchy — otherwise Discord will refuse role assignments
- The Simple Voice Chat mod is **not** required for Discord integration; it is only needed for the bag-mute feature

---

## Setting up the bot

### 1. Create an application

1. Go to [discord.com/developers/applications](https://discord.com/developers/applications) and click **New Application**
2. Give it a name (e.g. "Ivanica")
3. Open the **Bot** tab → click **Add Bot** → confirm

### 2. Copy the bot token

On the **Bot** tab click **Reset Token** and copy the value. You will paste this into `ivanica_discord.json`. Treat it like a password — anyone with this token controls your bot.

### 3. Get your guild (server) ID

In Discord, go to **User Settings → Advanced** and enable **Developer Mode**. Then right-click your server's icon in the sidebar and choose **Copy Server ID**. This is your `guildId`.

### 4. Invite the bot

Build an invite URL with the following OAuth2 scopes and permissions:

```
https://discord.com/api/oauth2/authorize
  ?client_id=YOUR_APPLICATION_ID
  &scope=bot+applications.commands
  &permissions=268435456
```

The permission integer `268435456` grants **Manage Roles** only. You can also use the OAuth2 URL Generator on the **OAuth2** tab: tick `bot` and `applications.commands` under Scopes, then tick **Manage Roles** under Bot Permissions.

Open the URL in a browser, select your server, and authorise.

### 5. Position the bot's role

After the bot joins, open **Server Settings → Roles** and drag the bot's role **above** all other roles you want it to manage. If the bot's role is below a nation role it cannot assign or remove that role and will log a 403 error.

---

## Configuring Ivanica

Edit `config/ivanica/discord.json` (created automatically on first server start):

```json
{
  "enabled": true,
  "botToken": "MTI3NjU2…",
  "guildId": "1234567890123456789"
}
```

| Field | Description |
|-------|-------------|
| `enabled` | `false` by default. Set `true` to activate all Discord features. |
| `botToken` | The raw token from the Developer Portal (not prefixed with `Bot `). |
| `guildId` | Numeric ID of the Discord server. |

A server restart is required after changing this file.

---

## Account linking

Discord role assignment only works for players who have linked their Minecraft account to a Discord user. Linking is voluntary — unlinked players are still full nation members in-game, they just won't receive Discord roles.

### Player flow

1. In Minecraft, run `/discord link`
   The game prints: `Run /link <CODE> in Discord to link your account.`
2. In Discord, run `/link <CODE>` (the `/link` slash command is registered automatically by the bot on startup)
3. The bot replies with a confirmation message, then deletes both the command invocation and its reply after 10 seconds
4. Any nation roles the player already belongs to are assigned immediately

Codes are 6 characters (uppercase letters and digits, no ambiguous characters), expire after **5 minutes**, and are one-time use. Running `/discord link` again generates a fresh code and invalidates the previous one.

Linked accounts are held **in memory only** — they are cleared on server restart. Players need to re-link after each restart.

---

## Nation roles

Every nation has exactly two Discord roles:

| Role | Name format | Who gets it |
|------|-------------|-------------|
| Member role | `NationName` | All nation members |
| Leader role | `NationName Leader` | Nation leader only |

Both roles are coloured with the nation's hex colour. The leader role is always positioned **above** the member role in the hierarchy (Ivanica swaps them if Discord returns them in the wrong order).

### Role lifecycle

| Event | What happens |
|-------|--------------|
| Nation created | Member role + Leader role created; leader assigned both (if linked) |
| Player joins nation | Assigned the member role (if linked) |
| Player leaves nation | Member role removed (if linked) |
| Player is kicked | Member role removed (if linked) |
| Nation renamed | Both roles renamed to match |
| Nation colour changed | Both roles recoloured |
| Nation deleted | Both roles deleted from Discord |
| Player links account | Any roles they already hold in-game are assigned immediately |

---

## Troubleshooting

**`[Discord] HTTP 403` in the server log**
The bot cannot manage a role. Check that the bot's role in Discord is above all nation roles.

**`/link` slash command not appearing in Discord**
The command is registered when the bot connects (READY event). Wait a few seconds after server start — Discord can take up to an hour to propagate guild commands globally, but in practice it's nearly instant. If it never appears, check that the invite URL included `applications.commands` scope.

**Roles not assigned on link**
The player may have typed the wrong code, or the code expired (5 minutes). Have them run `/discord link` again for a fresh code.

**Bot goes offline / server restarts**
While the Gateway is disconnected, no role changes are made. Changes that happened in-game during the outage (join/leave/kick) are not replayed — those players' Discord roles will be out of sync until they re-link or an admin corrects them manually. This is a known limitation of the in-memory link store.

**Nation roles are missing colours**
Discord requires the role colour to be provided as a decimal integer. Ivanica converts the nation's hex colour to decimal before sending — if the nation has no colour set, roles default to grey (`0`).
