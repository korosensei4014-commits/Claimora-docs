# Claimora

Lightweight land claiming for Paper 26.1.2 to 26.3. Players claim custom rectangles with a Claim Wand, pay
with claim blocks earned by playing, and see client-side 3D borders. Works for Java and Bedrock
(Geyser) players.

**📖 [Read the wiki](docs/README.md)**: [player guide](docs/players.md) ·
[server owner guide](docs/server-owners.md) · [developer API](docs/developers.md) ·
[changelog](docs/changelog.md)

## Features

- Rectangular claims picked with a wand, with a live preview of size and cost
- Claim blocks: a starting amount plus more for every hour of active play (and buyable with Vault)
- Trust levels (guest, containers, build, manager), claim bans, private subclaims, claim names, resizing by grabbing corners
- Claim options: block crop trampling, hostile mob spawning or PvP in your claim
- Claim Fly: fly inside your claims as a rank perk
- `/claim menu` to see, manage and teleport to your claims, as forms on Bedrock
- Client-side borders (PacketEvents): no entities, no lag, nothing to break
- Admin tools: look up anyone's claims, inspect, teleport, and an audit log
- Optional WorldGuard, PlaceholderAPI, Vault and Geyser support, optional Bedrock-style menus (the Claimora-BedrockMenus add-on with KoroUI), plus a developer API
- One-command import from GriefPrevention

## Requirements

- Paper 26.1.2, 26.2 or 26.3 (or a fork), Java 25
- [PacketEvents](https://modrinth.com/plugin/packetevents) 2.14.0 or newer

## Download

Download the latest `Claimora-<version>.jar` from the page where you got Claimora.
