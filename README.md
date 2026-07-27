<p align="center">
  <img src="docs/img/logo.png" alt="GSM logo" width="128">
</p>

# GSM — Gamna Server Manager

**English** | [한국어](README.ko.md)

A self-hosted web panel for installing, configuring, and monitoring Steam dedicated game servers.
SteamCMD download, server installation, config editing, live console logs — all from your browser, without ever opening a config file by hand.

> This repository is **for distribution only** (source is managed separately). Grab the executable from [Releases](../../releases).

<!-- DEMO GIF: once recorded, replace this comment with e.g. ![GSM demo](docs/img/demo.gif) -->

![Live console and server list](docs/img/en-01-console.png)

<table>
  <tr>
    <td width="50%"><img src="docs/img/en-04-newgame.png" alt="Pick a game and create a server"></td>
    <td width="50%"><img src="docs/img/en-02-settings.png" alt="Per-game settings, grouped and searchable"></td>
  </tr>
  <tr>
    <td align="center"><em>Pick a game — SteamCMD does the rest</em></td>
    <td align="center"><em>Every server setting, grouped and searchable</em></td>
  </tr>
  <tr>
    <td colspan="2"><img src="docs/img/en-03-mods.png" alt="Mod management"></td>
  </tr>
  <tr>
    <td colspan="2" align="center"><em>Mods from Steam Workshop and Thunderstore — no manual file copying</em></td>
  </tr>
</table>

## Why GSM?

- **Actually free — no tiers, no paywall.** Unlike **AMP** (paid per-install license) or a hosted **Pterodactyl** setup, GSM is free for both personal and commercial server hosting.
- **One executable, zero dependencies.** No Docker, no database, no PHP/Node stack to stand up like **Pterodactyl** requires — download, run, open your browser.
- **New games are just a JSON file.** Where **WindowsGSM** needs a per-game plugin/script, every game in GSM is one manifest, so behavior stays consistent as games are added.

## Download

👉 **[Download the latest release](../../releases/latest)** · [Changelog](CHANGELOG.md)

## Getting Started

1. Extract the release zip anywhere you like
2. Run `gsm.exe`
3. Open **http://127.0.0.1:8710** in your browser
4. `+ New server instance` → pick a game → Install → Start

SteamCMD and game server files are downloaded automatically on first install.

**First run on Windows:** GSM isn't code-signed, so Windows may show a **"Windows protected your PC"** SmartScreen warning the first time you run `gsm.exe`. Click **More info → Run anyway**.

Prefer to check the binary first? You can scan it yourself on [VirusTotal](https://www.virustotal.com/gui/home/upload).
VirusTotal for v0.5.16 — both binaries come back **clean, 0/66**: [Windows (gsm.exe)](https://www.virustotal.com/gui/file/921afdc7a27b60214a389c2d517f70276686f6dd4eb20f8e61ebf673c051ca12) · [Linux (gsm)](https://www.virustotal.com/gui/file/571b6c76208b9503d16ac2bd61b5c637385c2334495bb1a81f0dc89a6f98ce9b). (Antivirus signatures change over time, so an engine may later heuristically flag the unsigned Go binary — but Microsoft Defender and every major engine pass.)

## Supported Games

| Game | Notes |
|---|---|
| Project Zomboid | Admin password set automatically on first run |
| V Rising | |
| Valheim | Crossplay option supported |
| Palworld | Installs game files per server (~8GB each) — the game has no custom save-path support |
| Enshrouded | |
| Core Keeper | |
| Abiotic Factor | |
| Sons of the Forest | Public network-accessibility self-test skipped by default — turn it off in settings once you've port-forwarded a public server |
| Soulmask | |
| ARK: Survival Ascended | Map selection, difficulty/rates, RCON console + player kick/ban, CurseForge mods; installs per server (~13GB) |
| Conan Exiles | PvP/PvE, nudity level, XP rate, RCON console + player kick/ban, Steam Workshop mods; installs per server (~6GB) |
| Necesse | Top-down co-op survival & sandbox (bundled Java runtime); installs per server |
| Eco | Ecosystem-simulation co-op survival; offline mode by default; installs per server |
| Mordhau | Large-scale medieval melee PvP; RCON console + player kick/ban; installs per server (~5GB) |
| Unturned | Zombie survival; invite via Server Code; managed via in-game admin; installs per server |
| The Isle | Dinosaur survival (Evrima branch); managed via in-game admin; installs per server |

Games managed over a console or RCON (**Project Zomboid, Palworld, ARK, Conan, Mordhau**) get a **Players** tab — see who's connected and kick, ban or unban them in one click. The RCON ones (Palworld, ARK, Conan, Mordhau) also show a **live player count** (e.g. 3/16) on their server card. Games without a remote command channel are managed by **designating admins (SteamID) from the panel** or in-game (for V Rising, Valheim, Core Keeper and Abiotic Factor, edit the admin list under Settings → Advanced: edit config files directly).

Ship **Windows-only** dedicated servers (hidden on the Linux build): V Rising, Enshrouded, Core Keeper, Abiotic Factor, Sons of the Forest, Soulmask, ARK: Survival Ascended, Conan Exiles.

Want another game? File a [game request](../../issues/new/choose).

## Requirements & Notes

- Windows 10 or later (64-bit); experimental Linux build available (V Rising has no Linux server)
- Disk space: roughly 2–20 GB per game server
- For friends to join from outside, you need to port-forward on your router (per-game ports are shown in the panel's Settings tab). **Since v0.5.4, enabling UPnP in the ⚙ Settings** opens the game ports on your router automatically at server start (if your router supports and has UPnP enabled; the RCON port is never opened)
- 🔒 **Authentication is off by default (opt-in).** Out of the box there is no login and the panel binds to **localhost (127.0.0.1) only**. In that state, never port-forward or expose the panel port (**8710**) to the internet — anyone who can reach it can take control of every server GSM manages. For remote management, **set a password** in ⚙ Settings; enabling one lets you opt into **external access** and **HTTPS** (your own real cert·key), and non-local binding is blocked unless authentication is on. (Putting GSM behind an authenticating reverse proxy (Caddy/nginx with basic-auth or SSO) or a **VPN** also still works.) The *game* ports players connect to are separate and can be forwarded safely.

## Telemetry

Starting with v0.3.0, GSM sends **one anonymous ping per day** so development effort can go where it matters (which games, which languages). Exactly this and nothing more:

| Sent | Not sent — ever |
|---|---|
| Random anonymous ID, GSM version, OS (windows/linux), panel & browser language, installed game IDs, instance count, per-game start counts | Server names, passwords, settings values, file paths, player data, IP address (the collector derives a country code from the connection and discards the address) |

To disable, toggle it off in the panel's Settings (⚙, top right) or run with the `-no-telemetry` flag.

## Feedback

Bug reports · game requests · feature ideas → [Issues](../../issues/new/choose)

## Support the Project

If GSM saved you some pain, consider supporting development ☕

- [GitHub Sponsors](https://github.com/sponsors/popcorn-kim93)
- [Ko-fi](https://ko-fi.com/kangnengs)

---

🤖 GSM is built with AI-assisted development ([Claude](https://claude.com/claude-code) by Anthropic), directed and verified by a human developer.
