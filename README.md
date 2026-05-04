<p align="center">
  <img src="logo.png" alt="KellyMaster Logo" width="200"/>
</p>

<h1 align="center">KellyMaster</h1>

<p align="center">
  <strong>Advanced Anti-ForceOP Security Plugin for Minecraft Servers</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-2.0.0-red?style=for-the-badge" alt="Version"/>
  <img src="https://img.shields.io/badge/minecraft-1.17.1%20--%201.21.x+-green?style=for-the-badge" alt="Minecraft"/>
  <img src="https://img.shields.io/badge/folia-supported-blue?style=for-the-badge" alt="Folia"/>
  <img src="https://img.shields.io/badge/java-17+-orange?style=for-the-badge" alt="Java"/>
</p>

<p align="center">
  <a href="https://ko-fi.com/srcodexstudio">Website</a> &bull;
  <a href="#installation">Installation</a> &bull;
  <a href="#configuration">Configuration</a> &bull;
  <a href="#how-it-works">How It Works</a> &bull;
  <a href="#faq">FAQ</a>
</p>

---

## What is KellyMaster?

KellyMaster is a **premium-grade security plugin** (completely free) that protects your Minecraft server against **ForceOP attacks**, **unauthorized operator access**, and **privilege escalation exploits**.

When a player receives operator (OP) permissions, KellyMaster immediately **freezes** them and sends a **one-time verification code** to their pre-configured contact method (Email, Discord DM, or Telegram). Until they verify their identity, they cannot move, interact, chat, or execute any command.

If someone who is **not** in the operator whitelist gains OP (through exploits, backdoor plugins, or direct manipulation), KellyMaster instantly **removes their OP** and applies the configured sanction (ban, kick, or deop).

### Why KellyMaster?

- Servers get compromised daily through ForceOP exploits, backdoor plugins, and permission escalation
- A single unauthorized operator can destroy months of work in seconds
- Traditional security plugins only block commands — KellyMaster verifies **identity**
- Even if an attacker gets OP, they are **useless** without the verification code

---

## Features

### Multi-Layer Anti-ForceOP Defense

KellyMaster uses a **multi-layer detection system** that catches unauthorized operators through multiple independent mechanisms. Even if one layer is bypassed, the others provide backup protection with near-zero detection delay.

### Multi-Channel Verification

| Channel | Description |
|---------|-------------|
| **Email (SMTP)** | Sends verification codes via any SMTP provider (Gmail, Outlook, custom) |
| **Discord DM** | Sends codes directly to the operator's Discord account via bot |
| **Telegram** | Sends codes via Telegram bot with long-polling support |

All channels include professional, branded message templates with security details.

### Discord Webhook Alerts

Real-time security alerts sent to a Discord channel when:
- An unauthorized player attempts to gain OP
- A player tries to execute a blocked command
- Includes player name, IP, geolocation, ISP, and action taken

### LuckPerms Integration

Monitors LuckPerms permission changes in real-time. If an unauthorized player receives dangerous permissions (`*`, `essentials.*`, `luckperms.*`, `minecraft.command.op`, etc.), KellyMaster automatically:
- Removes the dangerous permissions
- Bans the player
- Alerts all authorized operators

### Operator Whitelist

Only players listed in the whitelist can hold OP permissions. Each operator is configured with their preferred verification method and contact information.

### Namespace-Aware Command Blocking

Blocks dangerous commands (`/op`, `/deop`, `/stop`, `/reload`, etc.) from being executed by players. Strips **any** namespace prefix — attempts like `/bukkit:op`, `/spigot:op`, `/essentials:op` are all caught.

### Plugin Protection

Prevents plugin management tools from unloading, disabling, or reloading KellyMaster. Covers PlugMan, PlugManX, ServerUtils, and other management tools.

### Complete Freeze System

During verification, operators are completely frozen:
- Cannot move (head rotation allowed)
- Cannot interact with blocks or entities
- Cannot open inventory
- Cannot chat
- Cannot drop items
- Cannot take or deal damage
- Cannot execute any command except `/kellymaster <code>`

### IP Geolocation

Enriches security alerts with geolocation data (country, city, ISP) for unauthorized access attempts. Uses a cached lookup system with minimal API calls.

### Multi-Language Support

External notifications (Email, Discord, Telegram) support 5 languages:
- English, Spanish, Russian, Chinese, Portuguese

In-game messages are fully customizable via `messages.yml`.

---

## Compatibility

| Platform | Versions | Status |
|----------|----------|--------|
| **Paper** | 1.17.1 — 1.21.x+ | Fully supported |
| **Folia** | All versions | Fully supported |
| **Spigot** | 1.17.1+ | Supported |
| **Java** | 17+ | Required |

### Optional Dependencies

| Plugin | Purpose |
|--------|---------|
| **LuckPerms** | Permission monitoring and dangerous permission detection |

---

## Installation

1. Download `KellyMaster.jar` from the [Releases](../../releases) page
2. Place it in your server's `plugins/` folder
3. Start (or restart) the server
4. Configure the plugin (see [Configuration](#configuration))
5. Restart the server to apply changes

On first startup, KellyMaster generates three configuration files in `plugins/KellyMaster/`:
- `config.yml` — Main plugin settings
- `whitelist.yml` — Operator whitelist and bot tokens
- `messages.yml` — All in-game messages (fully customizable)

---

## Commands

| Command | Description | Permission |
|---------|-------------|------------|
| `/kellymaster <code>` | Enter your verification code | `kellymaster.verify` (OP only) |
| `/kellymaster reload` | Reload configuration (console only) | Console only |

> **Note:** The `/kellymaster` command is the **only** command players can execute during verification. All other commands are blocked.

---

## Configuration

### config.yml — Main Settings

This file controls all plugin behavior. Below is a breakdown of each section.

#### General Settings

```yaml
settings:
  lang: "en"                       # Language for external notifications (en/es/ru/zh/pt)
  debug: false                     # Enable verbose logging
  verification-timeout: 60         # Seconds to enter the code (10-300)
  code-length: 8                   # Verification code length (4-12)
  deop-on-disconnect: true         # Remove OP when player disconnects
  block-offline-op: true           # Block /op for offline players
  max-verification-attempts: 3     # Failed attempts before kick (1-10)
```

| Setting | What it does |
|---------|-------------|
| `lang` | Changes the language of Email, Discord, and Telegram notifications. Does NOT affect in-game messages (those are in `messages.yml`) |
| `verification-timeout` | How many seconds an operator has to enter their code before being kicked |
| `code-length` | Length of the alphanumeric verification code. Longer = more secure |
| `deop-on-disconnect` | Automatically removes OP when a player disconnects, preventing retained OP on rejoin |
| `block-offline-op` | Prevents giving OP to players who are not currently online |
| `max-verification-attempts` | After this many wrong codes, the player is kicked and their OP is removed |

#### Server Information

```yaml
server:
  name: "Minecraft Server"    # Shown in verification messages
  ip: ""                      # Shown in verification messages (leave empty to hide)
```

This information appears in the verification emails, Discord DMs, and Telegram messages to help operators identify which server is requesting verification.

#### Security Actions

```yaml
security:
  unauthorized-action: BAN          # BAN / KICK / DEOP
  allow-non-whitelist-op: false     # Block OP for non-whitelisted players entirely
  log-ip-addresses: true            # Log IPs in security events
  log-operator-commands: true       # Log commands executed by operators
```

| Setting | What it does |
|---------|-------------|
| `unauthorized-action` | What happens to a player who gains OP but is NOT in the whitelist. **BAN** = permanent ban + kick. **KICK** = kick only. **DEOP** = silently remove OP |
| `allow-non-whitelist-op` | When `false`, the `/op` command is completely blocked for non-whitelisted players. When `true`, the command goes through but sanctions are applied immediately |

#### Blocked Commands

```yaml
blocked-commands:
  enabled: true
  commands:
    - "op"
    - "deop"
    - "stop"
    - "reload"
    - "restart"
    - "lp"
    - "luckperms"
```

These commands are **blocked for all players** and can only be executed from the server console. The blocking is namespace-aware — `/minecraft:op`, `/bukkit:op`, `/essentials:op` are all caught.

You can add any command to this list (without the `/`).

#### Discord Webhook

```yaml
webhook:
  enabled: false
  url: ""                           # Discord webhook URL
  notify-blocked-commands: true     # Alert when blocked commands are attempted
  notify-unauthorized-op: true      # Alert when unauthorized OP is detected
  include-ip: true                  # Include player IP in alerts
  include-geolocation: true         # Include country/city/ISP
```

**How to set up:**
1. Go to your Discord server → **Server Settings** → **Integrations** → **Webhooks**
2. Click **New Webhook**, choose a channel, and copy the URL
3. Paste the URL in the `url` field
4. Set `enabled: true`

#### Email (SMTP)

```yaml
email:
  enabled: true
  subject: "KellyMaster - Verification Code"
  timeout: 10000                    # SMTP timeout in milliseconds
  smtp:
    host: "smtp.gmail.com"          # SMTP server
    port: "587"                     # SMTP port (587 for TLS)
    user: ""                        # Your email address
    password: ""                    # Your email password or app password
```

**Gmail setup:**
1. Go to [Google App Passwords](https://myaccount.google.com/apppasswords)
2. Generate a new app password for "Mail"
3. Use your Gmail address as `user` and the generated password as `password`

**Other providers:**
| Provider | Host | Port |
|----------|------|------|
| Gmail | `smtp.gmail.com` | `587` |
| Outlook | `smtp-mail.outlook.com` | `587` |
| Yahoo | `smtp.mail.yahoo.com` | `587` |
| Custom | Your SMTP server | Your port |

#### Reload

```yaml
reload:
  enabled: false    # Disabled by default for security
```

The reload command is **disabled by default** for security reasons. When enabled, it can **only** be executed from the server console — never by a player, even with OP.

---

### whitelist.yml — Operator Whitelist

This is the most important file. It defines **who** can be an operator and **how** they verify their identity.

#### Discord Bot Setup

```yaml
discord:
  bot-token: ""       # Your Discord bot token
  enabled: false      # Set to true to enable
```

**How to create a Discord bot:**
1. Go to [Discord Developer Portal](https://discord.com/developers/applications)
2. Click **New Application** → give it a name → **Create**
3. Go to **Bot** tab → click **Reset Token** → copy the token
4. Under **Privileged Gateway Intents**, enable **Message Content Intent**
5. Go to **OAuth2** → **URL Generator** → check `bot` → check `Send Messages`
6. Use the generated URL to invite the bot to your server
7. Paste the token in `bot-token` and set `enabled: true`

> **Important:** The bot must be able to send Direct Messages to your operators. Make sure operators have DMs enabled for the server where the bot is present.

#### Telegram Bot Setup

```yaml
telegram:
  bot-token: ""       # Your Telegram bot token
  enabled: false      # Set to true to enable
```

**How to create a Telegram bot:**
1. Open Telegram and search for **@BotFather**
2. Send `/newbot` and follow the instructions
3. Copy the bot token (format: `123456789:ABCdefGHIjklMNopQRSTuvwxyz`)
4. Paste it in `bot-token` and set `enabled: true`

**How to get your Telegram Chat ID:**
1. Start a conversation with your bot on Telegram
2. Send `/start` to the bot
3. The bot will reply with your Chat ID

#### Verification Method Assignment

```yaml
setup-verification:
  - "PlayerName:Gmail"        # This player verifies via email
  - "AnotherPlayer:Discord"   # This player verifies via Discord DM
  - "ThirdPlayer:Telegram"    # This player verifies via Telegram
```

Each line maps a player name to their verification method. Available methods:
- `Gmail` (or `Email` / `Mail`) — sends code via SMTP email
- `Discord` — sends code via Discord DM
- `Telegram` — sends code via Telegram message

#### Authorized Operators

```yaml
authorized-operators:
  - "PlayerName:email@example.com"           # Email contact
  - "AnotherPlayer:123456789012345678"       # Discord User ID
  - "ThirdPlayer:987654321"                  # Telegram Chat ID
```

Each line maps a player name to their **contact information** for the verification method configured above.

| Method | Contact format | How to get it |
|--------|---------------|--------------|
| Email | `email@example.com` | Your email address |
| Discord | `123456789012345678` | Enable Developer Mode in Discord → Right-click user → Copy ID |
| Telegram | `987654321` | Send `/start` to your bot → it replies with your Chat ID |

> **Security:** Any player who gains OP and is NOT in this list will be automatically sanctioned (banned/kicked/deopped based on `config.yml`).

#### Ban Message

```yaml
ban-message:
  - "&c&l"
  - "&c&l    KELLYMASTER SECURITY"
  - "&cYou have been &4&lBANNED &cfrom this server"
  - "&7Reason: &fUnauthorized operator access"
  - "&7Player: &f{player}"
  - "&7Date: &f{date}"
  - "&7Time: &f{time}"
```

This message is shown to banned players when they try to rejoin. Supports `&` color codes and placeholders:

| Placeholder | Value |
|-------------|-------|
| `{player}` | The banned player's name |
| `{date}` | Ban date (dd/MM/yyyy) |
| `{time}` | Ban time (HH:mm:ss) |
| `{server}` | Server name |

---

### messages.yml — In-Game Messages

All in-game messages are fully customizable. The file is organized into sections:

| Section | Purpose |
|---------|---------|
| `prefix` | The plugin prefix shown before messages |
| `verification` | Messages during the verification process |
| `security` | Security alert messages |
| `blocked-command` | Messages when a blocked command is attempted |
| `command` | General command response messages |
| `reload` | Reload command messages |
| `titles` | Full-screen title messages |
| `actionbar` | Action bar messages |
| `kick` | Kick screen messages |

All messages support `&` color codes (`&a` = green, `&c` = red, `&e` = yellow, etc.) and `&#RRGGBB` hex colors.

**Placeholders available:**

| Placeholder | Where it works | Value |
|-------------|----------------|-------|
| `{time}` | Verification messages, titles | Seconds remaining |
| `{attempts}` | Failed code messages | Attempts remaining |
| `{player}` | Security messages | Player name |
| `{command}` | Blocked command messages | The blocked command |
| `{max}` | Kick messages | Maximum attempts |

---

## How It Works

### Verification Flow

```
1. Player gains OP (via /op command, ops.json, or plugin)
        ↓
2. KellyMaster detects the OP change instantly
        ↓
3. Player is FROZEN (cannot move, interact, chat, or use commands)
        ↓
4. A verification code is generated and sent to the operator's
   configured channel (Email / Discord / Telegram)
        ↓
5. The operator enters the code: /kellymaster <code>
        ↓
   ✅ Correct code → Player is unfrozen and can play normally
   ❌ Wrong code → Attempts decrease, player is warned
   ❌ Too many wrong codes → OP removed, player kicked
   ❌ Time expires → OP removed, player kicked
```

### Unauthorized Access

```
1. Non-whitelisted player gains OP (exploit, backdoor plugin, etc.)
        ↓
2. KellyMaster detects the unauthorized OP instantly
        ↓
3. OP is immediately removed
        ↓
4. Sanction is applied (BAN / KICK / DEOP based on config)
        ↓
5. All authorized operators are notified via their channels
        ↓
6. Discord webhook alert is sent (if configured)
```

---

## Performance

KellyMaster is designed for **zero impact** on server performance:

- All network operations (Email, Discord, Telegram, webhooks, geolocation) run on **background threads** — the server tick is never blocked
- The OP monitoring system uses less than **0.05%** of a single tick
- Memory footprint: **~15MB** without Discord, **~60MB** with Discord bot active
- Works on servers with as little as **2GB RAM**

---

## Security Notes

- **Never share** your bot tokens, SMTP passwords, or webhook URLs
- Keep `deop-on-disconnect: true` for maximum security
- Keep `allow-non-whitelist-op: false` to block unauthorized `/op` entirely
- Keep `reload.enabled: false` unless you specifically need hot-reload
- Regularly review the `banned-players` list in `whitelist.yml`
- Use a **firewall** to protect backend server ports if using BungeeCord/Velocity

---

## FAQ

**Q: Does KellyMaster work with BungeeCord/Velocity?**
A: Yes. KellyMaster runs on each backend server independently. For maximum security with proxies, use Velocity with modern forwarding or BungeeGuard.

**Q: What happens if the email/Discord/Telegram service is down?**
A: If the verification code cannot be delivered, the operator is notified in-game. They remain frozen until the code is delivered or the timeout expires. KellyMaster falls back to email if Discord or Telegram is unavailable.

**Q: Can I use multiple verification methods for different operators?**
A: Yes. Each operator can have their own method. One can use Email, another Discord, and another Telegram — all on the same server.

**Q: Does KellyMaster protect against PlugMan?**
A: Yes. KellyMaster blocks attempts to unload, disable, or reload itself through plugin management tools (PlugMan, PlugManX, ServerUtils, and others).

**Q: Is the source code open?**
A: No. KellyMaster is free to use but the source code is not open source. The plugin is distributed as an obfuscated JAR and receives regular security patches.

**Q: How often is KellyMaster updated?**
A: KellyMaster receives **weekly security patches** and feature updates. Check the [Releases](../../releases) page for the latest version.

---

## Support

- **Website:** [ko-fi.com/srcodexstudio](https://ko-fi.com/srcodexstudio)
- **Issues:** Use the [Issues](../../issues) tab to report bugs or request features
- **Author:** SrCodexStudio

---

<p align="center">
  <sub>Made with dedication by <strong>SrCodexStudio</strong></sub><br/>
  <sub>Protecting Minecraft servers, one operator at a time.</sub>
</p>
