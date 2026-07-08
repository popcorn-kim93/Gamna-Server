# GSM — Gamna Server Manager

**English** | [한국어](README.ko.md)

A self-hosted web panel for installing, configuring, and monitoring Steam dedicated game servers.
SteamCMD download, server installation, config editing, live console logs — all from your browser, without ever opening a config file by hand.

> This repository is **for distribution only** (source is managed separately). Grab the executable from [Releases](../../releases).

## Download

👉 **[Download the latest release](../../releases/latest)** · [Changelog](CHANGELOG.md)

## Getting Started

1. Extract the release zip anywhere you like
2. Run `gsm.exe`
3. Open **http://127.0.0.1:8710** in your browser
4. `+ New server instance` → pick a game → Install → Start

SteamCMD and game server files are downloaded automatically on first install.

## Supported Games

| Game | Notes |
|---|---|
| Project Zomboid | Admin password set automatically on first run |
| V Rising | |
| Valheim | Crossplay option supported |
| Palworld | Installs game files per server (~8GB each) — the game has no custom save-path support |

Want another game? File a [game request](../../issues/new/choose).

## Requirements & Notes

- Windows 10 or later (64-bit); experimental Linux build available (V Rising has no Linux server)
- Disk space: roughly 2–20 GB per game server
- For friends to join from outside, you need to port-forward on your router (per-game ports are shown in the panel's Settings tab)
- ⚠️ This version has no authentication — the panel binds to **localhost (127.0.0.1) only**. Do not expose it to the internet.

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
