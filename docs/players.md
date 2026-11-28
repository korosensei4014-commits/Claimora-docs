# Player guide

Claimora protects your builds. You mark out a rectangle of land with the **Claim Wand**, and
nobody else can break, build or open things there unless you trust them.

[← Back to the wiki](README.md)

## Contents

- [Your first claim](#your-first-claim)
- [Claim blocks](#claim-blocks)
- [What a claim protects](#what-a-claim-protects)
- [Trusting friends](#trusting-friends)
- [The claim menu](#the-claim-menu)
- [Resizing a claim](#resizing-a-claim)
- [Subclaims](#subclaims)
- [Claim options](#claim-options)
- [Claim Fly](#claim-fly)
- [Naming and renaming](#naming-and-renaming)
- [Teleporting to your claims](#teleporting-to-your-claims)
- [Giving a claim away](#giving-a-claim-away)
- [Abandoning a claim](#abandoning-a-claim)
- [Stuck in someone's claim](#stuck-in-someones-claim)
- [Banning players](#banning-players)
- [Border colours](#border-colours)
- [Bedrock players](#bedrock-players)
- [All commands](#all-commands)
- [FAQ](#faq)

## Your first claim

1. **Get the wand.** New players get it on their first join. Lost it? Type `/claim`.
2. **Hold it.** Claims near you light up with coloured borders, and the action bar shows how many
   claim blocks you have.
3. **Right-click a block** for the first corner.
4. **Look around.** The border follows your crosshair, and the action bar shows the size and cost.
5. **Right-click the opposite corner**, then **right-click once more** to confirm.

Left-click goes back a step if you change your mind.

> **In a hurry?** `/claim auto` claims a square around you in one go (11 × 11 by default).

Claims always cover the whole height of the world, from bedrock to the sky. You only choose the
two corners.

## Claim blocks

Land costs **claim blocks**: one block per square of ground. A 20 × 25 claim costs 500.

- Everyone starts with **100**.
- You earn **100 more for every hour you actively play** (standing AFK doesn't count).
- Abandoning a claim gives its blocks back.
- Subclaims are free, because they're inside land you already paid for.

See your balance with `/claim list`, in `/claim menu`, or on the action bar while holding the wand.

**Buying blocks:** if your server has an economy, `/claim buy` (or **Buy claim blocks** in
`/claim menu`) lets you buy more, for example `/claim buy 500`. You see the price and confirm first.
You see your money, the price and what you'll have afterwards before you confirm. Bought blocks are
yours for good.

*(These are the default numbers. Your server may have changed them.)*

## What a claim protects

Inside your claim, players you haven't trusted **can't**:

- break or place blocks, use buckets, or edit signs
- open chests, barrels, furnaces and other containers
- use doors, trapdoors, gates, buttons, levers or pressure plates
- hurt animals, or break item frames, paintings, armor stands or vehicles

Claims also stop explosions, fire spread, and pistons, water or lava pushing in from outside.
Crafting tables and ender chests stay usable by everyone, and villager trading is allowed (your
server can change both).

If you try to build somewhere you can't, the claim's border flashes so you can see whose land it is.

## Trusting friends

Stand in your claim and:

| Command | What it does |
| --- | --- |
| `/trust` | Pick an online player from a list, then pick their level |
| `/trust <player>` | Trust someone at **Build**, even if they're offline |
| `/trust <player> <level>` | Trust someone at a level: `guest`, `containers`, `build` or `manager` |
| `/untrust <player>` | Remove them |
| `/trustlist` | See who's trusted and at which level. Tap someone to change their level or remove them. |

**Levels** (each one includes everything above it):

| Level | They can |
| --- | --- |
| **Guest** | Use doors, trapdoors, gates, buttons, levers, pressure plates, beds and bells, and ride vehicles |
| **Containers** | Guest, plus open chests and other storage, look after animals (feed, breed, shear, milk) and farm: harvest ripe crops and replant them |
| **Build** | Containers, plus break and place anything |
| **Manager** | Build, plus trust and untrust other players (up to Build) |

Only you, the owner, can resize, rename, change options, give away or abandon the claim. Managers
can't make or remove other Managers.

Trusted players at every level can teleport to and Claim-Fly in your claim, unless the server
requires a higher level.

## The claim menu

`/claim menu` opens a menu (a form on Bedrock) with:

- **Your claims**: pick one to **Teleport** or **Manage** it
- **Trusted claims**: claims other players trusted you in
- **Help**

**Manage** lets you change trusted players, subclaims, the name and the home point, give the claim
away, or abandon it, all without walking there.

If the server has Claimora's **Bedrock-style menus** and you accepted the resource pack, `/claim menu` looks
like Bedrock's own menus on Java too: a banner with your blocks left, rows for Your claims, Trusted claims, Buy
blocks and Help, and a **Get claim wand** button. Each claim has its own page with Teleport, Set home here,
Rename, switches for its options (they save straight away), Trusted players, Banned players, Subclaims, Transfer
and Abandon. Trust levels, buying blocks (a slider), bans, renaming and the confirmations use the same style.
Rename and "ban an offline player" use Minecraft's own text box under the page. Long lists (many claims or
players) have **< Previous** and **Next >** buttons at the bottom.

Bedrock players, and Java players without the pack, see the normal menus with the same options.

The normal menu (without the Bedrock-style menus, or on Bedrock) now also has **Get claim wand**, and its **Buy claim
blocks** screen has a slider for any amount, with the usual pack buttons below it.

## Resizing a claim

Hold the wand, **right-click a corner** of your claim (the border shows where the corners are),
move to the new spot, and right-click again. Growing a claim costs the extra blocks. Shrinking it
gives blocks back.

## Subclaims

A subclaim is a smaller area inside your claim with its **own trust list**. For example, a storage
room only some friends can open.

- Hold the wand and **right-click inside your claim** to start one, then pick the corners as usual.
- Subclaims are **private**: only you and the players trusted *in that subclaim* can build there.
  Players trusted in the main claim can't, unless you trust them in the subclaim too.
- Stand inside a subclaim and use `/trust` to manage its list.
- Subclaims have yellow borders.

## Claim options

Each claim has a few settings you can change: `/claim options` while standing in it, or
**Manage → Options** in `/claim menu`. Tap a setting to switch it.

| Option | What it does |
| --- | --- |
| **Crop trampling** | *Blocked*: nobody can trample farmland in the claim, not even you or your trusted players. (Mobs and strangers can never trample your crops anyway.) |
| **Hostile mobs** | *Don't spawn*: zombies, skeletons, creepers and other hostile mobs stop spawning naturally in the claim. Mob spawners still work, and mobs can still walk in from outside. |
| **PvP** | *Off*: players can't hurt each other in the claim, including with arrows and harmful potions. |
| **Claim Fly** | *Blocked*: nobody can use `/claimfly` in the claim, not even you. Handy if you trust friends to build but don't want them flying around, or for a parkour subclaim. |

- Subclaims start with their main claim's options and keep following it until you change them.
  Stand in a subclaim and use `/claim options` (or **Manage → Options** on the subclaim) and tap a
  setting to switch it just there. Setting it back to match the main claim makes it follow the main
  claim again. For example, block hostile mobs across your
  base but allow them in a "Mob farm" subclaim, or turn PvP off everywhere except an arena subclaim.
- Only the owner can change them. Trusted players can't.
- Your server may have turned some of these off. They then don't appear in the menu.

## Claim Fly

If your rank includes it, `/claimfly` (or `/claim fly`) lets you fly inside claims you own or are
trusted in.

- Type `/claimfly`, then double-jump inside your claim to fly. `/claimfly` again turns it off.
- **Daily time:** depending on your rank you get a number of flying minutes per day (or unlimited).
  Only time spent actually flying counts: standing or walking with Claim Fly on is free. The time
  resets every day at the same time (midnight Philippine time by default).
- While you fly, the action bar shows **✈ Claim Fly · 42:17 left**. You get a chat warning at 5 and 1
  minutes left. When your time runs out mid-air you get the same 3-second countdown and a safe fall.
- **Leaving the claim while flying:** you get a 3-second countdown (*flight ends in 3…*). Fly back in
  and you keep flying. Otherwise flight stops and you fall, but **that fall doesn't hurt**.
- **Walking out:** flight pauses and comes back when you walk into an allowed claim again.
- **PvP:** hitting another player, or being hit by one, turns Claim Fly off at once (your fall is
  still safe), and you can't turn it back on for 10 seconds. Mobs don't count.
- `/claimfly info` shows whether it's on, your rank tier, whether you can fly where you stand, your
  time left today and when it resets.
- Claim owners can block Claim Fly in their claim (see [Claim options](#claim-options)).

*(Your server decides who gets Claim Fly, whether trusted claims count, and the timings.)*

## Naming and renaming

New claims ask for a name (for example "Base"). Skip it and the claim is called "Claim 1",
"Claim 2" and so on. Each of your claims needs a different name.

- `/claim rename` opens the name box for the claim you're standing in.
- `/claim rename <name>` renames it straight away.

## Teleporting to your claims

| Command | What it does |
| --- | --- |
| `/claim tp` | Opens the menu so you can pick a claim |
| `/claim tp Base` | Teleports to your claim called "Base" |
| `/claim tp Base/Storage` | Teleports to the subclaim "Storage" inside "Base" |
| `/claim sethome` | Sets where you land in the claim you're standing in |

You stand still for a few seconds while dots fill up in the action bar. Moving or taking damage
cancels it. After a teleport there's a short cooldown.

Without a home point, you land on a safe spot near the middle of the claim. You can also teleport
to claims you're trusted in, if the server allows it.

Some servers turn claim teleport off; then `/claim tp` says so and the Teleport buttons aren't in the menus.

## Giving a claim away

Stand in the claim and type `/claim transfer <player>`, or use **Manage → Transfer** in the menu.
The new owner must be online and pays the claim blocks. You get yours back. Trusted players and
subclaims stay with the claim.

## Abandoning a claim

`/claim abandon` (or **Manage → Abandon**) deletes the claim or subclaim you're standing in, after
a confirmation. You get its claim blocks back. **This can't be undone.**

## Stuck in someone's claim

If you're trapped inside another player's claim and can't build your way out, type
`/claim trapped`. Stand still for 10 seconds (moving or getting hurt cancels it) and you're moved to
the nearest safe spot just outside the claim. You can use it once an hour. If no safe spot is found
nearby, nothing happens and you can ask an admin.

## Banning players

If someone keeps bothering you, ban them from your claim: stand in it and type `/claim ban <player>`, or open
**Banned players** on the claim's page in `/claim menu`.

- A banned player can't enter the claim or any of its subclaims, by any route: walking, flying, riding, ender
  pearls (they get the pearl back) or teleport commands. Walking into it feels like a wall, with a short message
  and the border shown for a moment. If they're inside when you ban them, they're moved just outside.
- Banning someone also removes their trust in that claim and its subclaims. Unbanning doesn't bring their trust
  back; trust them again if you want.
- Only the claim's owner can ban. You can't ban admins.
- A claim can hold up to 50 bans (the server may change this). Offline players can be banned by name.
- `/claim unban <player>` lifts a ban, `/claim bans` lists them. Tapping a name in the menu also unbans.

Some servers turn bans off; then these commands and the menu row aren't there.

## Border colours

| Colour | Meaning |
| --- | --- |
| Green | Your claim |
| Red | Someone else's claim |
| Yellow | A subclaim |
| Orange | A server (admin) claim, such as spawn |
| Light blue | Your selection, and it's valid |
| Red selection | Your selection overlaps something or is too small |

## Bedrock players

Everything works on Bedrock too. Menus and the name box appear as forms. Borders show either as
coloured particles or as the same 3D beams Java players see, depending on the server.

## All commands

| Command | What it does |
| --- | --- |
| `/claim` | Get the Claim Wand |
| `/claim menu` | Your claims, trusted claims, teleport and managing |
| `/claim list` | Your claim blocks and claims, in chat |
| `/claim auto` | Claim a square around you in one step |
| `/claim tp [name]` | Teleport to a claim (`Base/Storage` for a subclaim) |
| `/claim sethome` | Set where `/claim tp` lands in this claim |
| `/claim options` | Crop trampling, hostile mobs and PvP for this claim |
| `/claim rename [name]` | Rename the claim you're standing in |
| `/claim transfer <player>` | Give the claim you're standing in to an online player |
| `/claim abandon` | Delete the claim or subclaim you're standing in |
| `/claimfly` | Fly inside your claims (rank perk) |
| `/claimfly info` | Claim Fly status, tier, time left today and when it resets |
| `/claim buy [amount]` | Buy claim blocks (if the server has an economy) |
| `/claim trapped` | Get out of someone else's claim you're stuck in |
| `/claim ban [player]` | Ban a player from the claim you're standing in (no name: pick from a menu) |
| `/claim unban <player>` | Lift a ban |
| `/claim bans` | See and lift the bans of the claim you're standing in |
| `/claim help` | Short help in chat |
| `/trust [player]` | Let a player build here |
| `/untrust <player>` | Remove a trusted player |
| `/trustlist` | See and remove trusted players |

## FAQ

**Why can't I claim here?** The usual reasons: you've reached your rank's claim limit, you're too close to another claim (claims can't
overlap), the area is too small (at least 5 wide and 25 blocks by default), you don't have enough
claim blocks, claiming is turned off in this world, or the land is a protected server region.
The action bar tells you which one.

**Do I lose my claims if I stop playing?** Only if your server turned on inactive-claim expiry.
Ask an admin. Expired claims can be restored by an admin for a while afterwards.

**I abandoned my base by accident.** Abandons can't be undone, so ask an admin. They can see who
owned what, but can't bring an abandoned claim back.

**What are orange claims?** Land the server owns, such as spawn or a market. Only players an admin
trusted there can build. They don't belong to any player.

**Can I make a claim go up only to a certain height?** No. Claims always cover the full height.
