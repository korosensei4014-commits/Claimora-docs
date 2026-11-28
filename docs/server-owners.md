# Server owner guide

How to install, configure and run Claimora, and what the admin tools do.

[← Back to the wiki](README.md)

## Contents

- [Installing](#installing)
- [Updating](#updating)
- [Permissions](#permissions)
- [Admin commands](#admin-commands)
- [Looking up a player's claims](#looking-up-a-players-claims)
- [Buying claim blocks](#buying-claim-blocks)
- [Claim limits](#claim-limits)
- [/claim trapped](#claim-trapped)
- [Claim bans](#claim-bans)
- [Trust levels](#trust-levels)
- [Claim Fly](#claim-fly)
- [Admin claims (server land)](#admin-claims-server-land)
- [The admin log](#the-admin-log)
- [Configuration](#configuration)
- [Messages](#messages)
- [Bedrock (Geyser) support](#bedrock-geyser-support)
- [Bedrock-style menus (Claimora-BedrockMenus)](#bedrock-style-menus-claimora-bedrockmenus)
- [WorldGuard](#worldguard)
- [PlaceholderAPI](#placeholderapi)
- [Inactive-claim expiry](#inactive-claim-expiry)
- [Moving from GriefPrevention](#moving-from-griefprevention)
- [Usage statistics (bStats)](#usage-statistics-bstats)
- [Update checker](#update-checker)
- [Data and backups](#data-and-backups)
- [Performance and memory](#performance-and-memory)
- [Troubleshooting](#troubleshooting)

## Installing

**You need:**

- Paper 26.1.2, 26.2 or 26.3, or a fork (Purpur, Universe…), on Java 25. The Bedrock-style menus add-on
  needs KoroUI 0.3.3 or newer on 26.1.2 and 26.2 (older KoroUI only loads on 26.3). Plain Spigot and Folia aren't supported: Claimora says so
  in the console at startup and switches itself off.
- [PacketEvents](https://modrinth.com/plugin/packetevents) 2.14.0 or newer. This is required: it
  draws the claim borders.

**Optional:**

- Geyser + Floodgate, for Bedrock players
- WorldGuard 7, so players can't claim inside your regions
- PlaceholderAPI, for `%claimora_…%` placeholders
- Vault plus an economy plugin (EssentialsX, CMI…), for `/claim buy`

**Steps:**

1. Download the latest `Claimora-<version>.jar` from the page where you got Claimora.
2. Put it and PacketEvents in `plugins/`.
3. Start the server. Claimora creates `plugins/Claimora/` with `config.yml`, `messages.yml` and
   `claimora.db`.

## Updating

Replace the jar and restart. You **don't** need to delete `config.yml` or `messages.yml`. New
settings and messages use their defaults until you add them to your files. The database upgrades
itself on first start, and the console says so (for example "Database updated to schema version 4").

To see every option of a new version, rename your old `config.yml` and `messages.yml`, let Claimora
make fresh ones on the next start, and compare them.

## Permissions

| Permission | Default | Grants |
| --- | --- | --- |
| `claimora.use` | everyone | Claim land, trust players, use the wand and menus |
| `claimora.teleport` | everyone | `/claim tp` |
| `claimora.teleport.nocooldown` | op | Skip the `/claim tp` cooldown |
| `claimora.claims.max.<n>` | nobody | Can own at most n main claims (highest wins). Without it: no limit. |
| `claimora.trapped.nocooldown` | op | `/claim trapped` without the cooldown |
| `claimora.ban` | everyone | Ban and unban players from claims you own (`/claim ban`, `unban`, `bans`) |
| `claimora.ban.bypass` | op | Can't be banned and walks through claim bans (part of `claimora.admin`) |
| `claimora.fly.tier.<name>` | nobody | Claim Fly with that tier from `claim-fly.tiers` (see [Claim Fly](#claim-fly)) |
| `claimora.admin.view` | op | `/claimadmin claims`, `inspect` and `log`: look at anyone's claims |
| `claimora.admin.teleport` | op | `/claimadmin tp`: teleport to any claim instantly |
| `claimora.admin.modify` | op | Trust, rename, transfer and resize anyone's claims; make and manage admin claims. Includes view. |
| `claimora.admin.delete` | op | `/claimadmin delete` and `restore`. Includes modify and view. |
| `claimora.admin.updates` | op | Be told on join when a newer Claimora is out (see [Update checker](#update-checker)) |
| `claimora.admin` | op | Everything above (including `updates`), plus `blocks`, `bypass`, `reload` and `memory` |

**Teleport is separate on purpose.** View, modify and delete don't include it, so you can let a
helper look up claims without letting them travel to everyone's base. Give both if you want both.

**Example setup with LuckPerms:**

```
/lp group helper permission set claimora.admin.view true
/lp group moderator permission set claimora.admin.modify true
/lp group moderator permission set claimora.admin.teleport true
/lp group admin permission set claimora.admin true
```

Permissions are checked again on every menu click, so a menu opened before someone was demoted
can't be used afterwards.

## Admin commands

All admin commands are under `/claimadmin`. A claim **ID** is the number shown in inspect and in
claim lists (`12` or `#12` both work). IDs never change, unlike a claim's position in a list.

| Command | Permission | What it does |
| --- | --- | --- |
| `/claimadmin claims <player>` | view | That player's claims, online or offline, as a menu |
| `/claimadmin claims server` | view | Every admin (server) claim |
| `/claimadmin adminclaim` | modify | Your next wand claim (within 2 minutes) becomes an admin claim |
| `/claimadmin inspect [id]` | view | Claim details. No ID: the claim you're standing in. |
| `/claimadmin tp <id>` | teleport | Teleport to any claim or subclaim. No warm-up or cooldown. |
| `/claimadmin tp <id> center` | teleport | Same, but ignore the owner's home point and land near the centre |
| `/claimadmin trust <id> <player> [level]` | modify | Trust a player in any claim or subclaim (default Build) |
| `/claimadmin untrust <id> <player>` | modify | Remove a trusted player |
| `/claimadmin rename <id> <name>` | modify | Rename any claim |
| `/claimadmin transfer <id> <player>` | modify | Give a main claim (with its subclaims) to anyone, even offline. Asks to confirm. |
| `/claimadmin transfer <id> server` | modify | Turn a player's claim into an admin claim |
| `/claimadmin resize <id>` | modify | Arms your wand: right-click a corner of that claim within 2 minutes |
| `/claimadmin delete [id]` | delete | Delete a claim (no ID: the one you're standing in). Asks to confirm. |
| `/claimadmin restore` | delete | List the recycle bin (newest 50). Tap an entry to restore it. |
| `/claimadmin restore <id>` | delete | Bring a deleted claim back |
| `/claimadmin restore <player>` | delete | That player's claims in the recycle bin |
| `/claimadmin restore purge <id\|admin\|expired\|all>` | delete | Remove recycle-bin entries for good: one claim, all admin deletes, all inactivity removals, or everything. Asks first; add `confirm` to go ahead. |
| `/claimadmin log` | view | The newest 50 admin actions |
| `/claimadmin log <player>` / `log #<id>` | view | Admin actions on one player's claims, or one claim |
| `/claimadmin blocks give <player> <n>` | admin | Add claim blocks (works offline) |
| `/claimadmin blocks take <player> <n>` | admin | Remove claim blocks |
| `/claimadmin blocks set <player> <n>` | admin | Set how many claim blocks the player has **left to spend** (their claims keep the blocks they use). Before 0.8.5 this set the total, so `set 0` could leave a player at a negative number. |
| `/claimadmin bypass` | admin | Toggle ignoring all claims yourself |
| `/claimadmin reload` | admin | Reload `config.yml` and `messages.yml` |
| `/claimadmin import griefprevention [confirm]` | admin | Copy GriefPrevention's claims, trust and claim blocks (see [Moving from GriefPrevention](#moving-from-griefprevention)) |
| `/claimadmin memory` | admin | Show the size of everything Claimora keeps in memory |

Everything except `tp`, `resize` and `bypass` also works from the console, with text output. From
the console, `transfer` and `delete <id>` happen straight away without a confirmation.

**Admin changes ignore claim blocks.** Admin resize and transfer don't check the owner's balance,
so you can fix things a player couldn't afford. The new size still counts as the owner's used
blocks, so their available balance can go negative. That only stops them making new claims until
they earn more.

**Resizing someone's claim:** run `/claimadmin resize <id>` (or press **Resize** in inspect), hold
the Claim Wand, right-click a corner of that claim, move to the new spot and right-click twice, as
a player would. Left-click cancels. The action bar shows "Admin resize" while it's armed.

Players with the modify permission can also use `/trust` and `/untrust` while standing in anyone's
claim, and the admin log records it.

## Looking up a player's claims

`/claimadmin claims Steve` opens Steve's claims:

```
Steve's claims
  4 claims · 1,250 blocks used · last seen 12 days ago
  [ Base #12 · 2 subclaims ]  [ Farm #31 · 0 subclaims ]  …
```

Tap a claim to **inspect** it. You'll see the owner (name and UUID), world, corners, size, creation
date, home point, trusted players and subclaims. From there you can **Teleport** (teleport
permission), open its **Subclaims**, and, with the modify permission, change **Trusted players**,
**Rename**, **Transfer** (to an online player, with a confirmation) or **Resize** it.

If another admin changes the claim's owner while your transfer dialog is open, the confirm button
stops with a message instead of applying an old choice.

- Offline players work. The name is looked up in the server's own player cache (anyone who has
  joined before), never online.
- A raw UUID works too: `/claimadmin claims 069a79f4-44e9-4726-a5be-fca90e38aaf5`.
- Long lists are split into pages of 36.
- **Tab completion** suggests online players and offline players who own claims (up to 30, matching
  what you've typed). It only uses names the server still remembers in memory, so nothing is read
  from disk. A player missing from the suggestions still works if you type the full name.

**Where admin teleport lands:** the claim's home point if it's safe, otherwise a safe spot near the
centre inside the claim. In the Nether it searches below the roof. If nothing safe is found it
tells you instead of dropping you somewhere dangerous. Use `center` to skip the home point (for
example, if an owner hid it underground).

## Buying claim blocks

With Vault and an economy plugin installed, players can buy claim blocks: `/claim buy <amount>`, or
**Buy claim blocks** in `/claim menu` (preset amounts, handy on Bedrock). Without Vault or an economy,
the command says buying isn't available and the menu button is hidden.

- **Price:** amount × `price-per-block` (default 2.0, so 500 blocks cost 1,000). The player sees the
  price and their balance and confirms.
- **Safe order:** the money is taken first; the blocks are added only if the payment succeeded.
  Every purchase is printed to the console.
- **Limits:** `min-amount` and `max-per-purchase` per purchase, and `max-bought` per player over
  their lifetime (0 = no limit).
- Bought blocks are kept for good and counted separately (so `max-bought` works). There's no
  selling back, on purpose: otherwise blocks earned by playing could be turned into money.

| Key | Default | Meaning |
| --- | --- | --- |
| `claim-blocks.buy.enabled` | true | Allow buying (still needs Vault + an economy) |
| `claim-blocks.buy.price-per-block` | 2.0 | Price of one block |
| `claim-blocks.buy.min-amount` | 10 | Smallest purchase |
| `claim-blocks.buy.max-per-purchase` | 5000 | Largest purchase |
| `claim-blocks.buy.max-bought` | 50000 | Lifetime limit per player (0 = none) |
| `claim-blocks.buy.presets` | 100, 500, 1000 | Amounts offered in the menu |

## Claim limits

Give a rank `claimora.claims.max.<n>` to cap how many **main claims** its players can own:

```
/lp group default permission set claimora.claims.max.3 true
/lp group vip permission set claimora.claims.max.10 true
```

- With several, the **highest** number counts. **No permission means no limit**; claim blocks still
  limit the total area.
- Subclaims and admin claims don't count.
- At the limit, starting a new claim, `/claim auto` and receiving a claim by `/claim transfer` are
  refused with a message. Players already over the limit (after a rank change) keep their claims.
- Admin transfers ignore limits.

## /claim trapped

For players stuck inside someone else's claim. Only works where they can't build. They stand still
for `trapped.warmup-seconds` (10; moving or damage cancels), then go to the nearest safe, unclaimed
spot just outside the claim's closest border (within 64 blocks; only that border's chunk is loaded).
If nothing safe is found they stay put and are told to ask an admin; the cooldown isn't used. Cooldown:
`trapped.cooldown-minutes` (60), kept across reconnects; `claimora.trapped.nocooldown` skips it.

## Claim bans

Claim owners can ban players from a claim (`/claim ban <player>`, or **Banned players** on the claim's page).
A banned player can't get into the claim or its subclaims: moves into it are cancelled at the border, ender
pearls and chorus fruit landing inside are cancelled (the pearl is given back), riders (horses, pigs, boats, minecarts and other mounts) are dismounted and moved out with their mount, and
anyone who arrives inside by command, plugin, portal, respawn or login is moved to the nearest safe spot
outside. If none is found, they go to their bed or respawn anchor (if it's valid and not in a claim they're
banned from), otherwise the world spawn. Banning also removes the player's trust in that claim and its
subclaims.

- Only the owner bans. Admins with `claimora.admin.modify` can manage bans on any claim; their changes go into
  the admin log. Players with `claimora.ban.bypass` (admins by default) can't be banned and aren't stopped.
- Other plugins' teleports aren't cancelled (that could break `/home`, `/warp` and the like); the player is
  moved out right after they arrive.
- Config (`claim-ban`): `enabled` (default true), `max-per-claim` (50) and `message-cooldown-seconds` (3).
  The sound is `sounds.banned`. With `enabled: false` there are no ban commands, menu rows or checks, and no
  listener is registered; stored bans are kept and come back when you turn it on again (a reload is enough).
- Bans are stored in the `claim_bans` table (database schema 10, added automatically on first start; nothing
  existing changes). They follow the claim through transfers, resizes and the recycle bin.

## Trust levels

Players trust others at one of four levels with `/trust <player> [level]` (or the menus). Each level
includes the ones before it:

| Level | Allows |
| --- | --- |
| `guest` | Doors, trapdoors, fence gates, buttons, levers, pressure plates, beds, bells, riding vehicles |
| `containers` | Guest + chests, barrels, furnaces, hoppers and other storage, lecterns, composters, animals (interact and kill), farming (harvest ripe crops, melons and pumpkins; replant; bone meal) |
| `build` | Containers + everything else. Plain `/trust <player>` gives this. |
| `manager` | Build + trusting and untrusting others up to Build |

- Upgrading from an older version: everyone already trusted becomes **Build**, which is what they
  had, so nothing changes until owners pick other levels.
- Subclaims have their own trust lists with the same levels, and stay private.
- `teleport.min-trust` and `claim-fly.min-trust` (default `guest`) set the lowest level that may
  teleport to, or Claim-Fly in, a trusted claim.
- Admins: `/claimadmin trust <id> <player> [level]`.

## Claim Fly

`/claimfly` lets players fly inside claims, as a rank perk. Nobody has it by default, ops included.

**Giving it to a rank:** each tier in `claim-fly.tiers` is a permission `claimora.fly.tier.<name>`:

```
/lp group elite permission set claimora.fly.tier.elite true
/lp group sovereign permission set claimora.fly.tier.sovereign true
```

If a player has several tiers, the one with the largest allowance counts (`unlimited` beats
everything). Rank changes during the day apply at once: time already flown still counts.

**Daily time:**

- Only time **actually flying** counts. Standing or walking with Claim Fly on, and the grace
  countdown, are free.
- The allowance resets at `reset-time` in `timezone` (default 00:00 `Asia/Manila`). Use IANA time
  zone IDs (`UTC`, `America/New_York`, `Europe/London`…); daylight saving is handled.
- There's no reset task. Each player stores the time used and which day it belongs to; a stored
  day that isn't today counts as nothing used. Restarts, crashes and offline days don't matter, and
  reconnecting, changing worlds or switching claims can't reset anyone's time.
- Time is saved whenever a flight ends (landing, leaving, toggling off), on quit and on shutdown.
  After a crash, at most the flight in progress is lost.
- Warnings at `warn-at` (5m, 1m). Running out mid-air gives the grace countdown and a safe fall.

**Admin commands:**

| Command | Permission | What it does |
| --- | --- | --- |
| `/claimadmin flytime <player>` | view | Tier, used and left today (online players) |
| `/claimadmin flytime <player> add <time>` | modify | Extra time for today only (works offline) |
| `/claimadmin flytime <player> set <time>` | modify | Set today's time left (online players: it depends on their rank) |
| `/claimadmin flytime <player> reset` | modify | Give back today's full allowance (works offline) |

All are logged in the admin log. `%claimora_fly_remaining%` shows time left (`42:17`, `unlimited`,
or empty without a tier).

**Where:** claims the player owns, and (with `where: own-and-trusted`, the default) claims or
subclaims they're trusted in. Admin claims only with `in-admin-claims: true`, for players trusted there.

**How it behaves:**

- Flying out of an allowed claim starts a `grace-seconds` (3) countdown in the action bar. Flying back
  in cancels it. When it ends, flight stops and the player's next fall does no damage.
- Walking out pauses flight quietly; it comes back inside an allowed claim.
- With `disable-on-pvp: true`, real player-vs-player damage (not mobs, not blocked hits) turns Claim Fly
  off for both players straight away, with a safe fall, and locks it for `pvp-cooldown-seconds` (10).
- Losing the tier, the claim being deleted, the player being untrusted, or the owner blocking the
  `claim-fly` option all stop flight the same way.
- Quit, death, switching to creative or spectator, and the plugin shutting down all end
  Claim Fly cleanly.

**Other flight plugins:** Claimora only turns off flight it turned on. Players with a permission in
`own-flight-permissions` (default `essentials.fly`) already fly, so `/claimfly` leaves them alone. If
another plugin (a combat tag, say) turns flight off, Claim Fly notices and switches itself off.

| Key | Default | Meaning |
| --- | --- | --- |
| `claim-fly.enabled` | true | Turn Claim Fly on or off for the whole server. Off: `/claimfly`, its timer and the `claim-fly` claim option all disappear. |
| `claim-fly.timezone` | Asia/Manila | Time zone for the daily reset (IANA ID) |
| `claim-fly.reset-time` | 00:00 | Local time of the daily reset |
| `claim-fly.warn-at` | 5m, 1m | Chat warnings when this much time is left |
| `claim-fly.where` | own-and-trusted | `own`, or `own-and-trusted` |
| `claim-fly.min-trust` | guest | Lowest trust level that may fly in a trusted claim |
| `claim-fly.in-admin-claims` | false | Allow it in admin claims (trusted players only) |
| `claim-fly.grace-seconds` | 3 | Countdown after flying out of an allowed claim |
| `claim-fly.show-timer` | true | "✈ Claim Fly · 42:17 left" in the action bar while flying |
| `claim-fly.disable-on-pvp` | true | PvP turns Claim Fly off |
| `claim-fly.pvp-cooldown-seconds` | 10 | How long it stays locked after PvP |
| `claim-fly.own-flight-permissions` | essentials.fly | Players with these already fly; left alone |
| `claim-fly.tiers` | elite 30m, royal 60m, emperor 120m, sovereign unlimited | Tier names and daily time (`30s`, `15m`, `1h30m`, `unlimited`) |

## Admin claims (server land)

Admin claims protect land the server owns: spawn, a market, event areas. They have **orange**
borders and show "Server" as the owner.

**Making one:** run `/claimadmin adminclaim`, hold the Claim Wand and select the area as usual
within 2 minutes. The action bar says "Admin claim" while it's armed. You'll be asked for a name.
Left-click cancels.

**Turning an existing claim into one:** `/claimadmin transfer <id> server`, or **Transfer → Server**
in inspect. `transfer <id> <player>` gives it back to a player (they don't pay claim blocks).

**How they behave:**

- **Free:** no claim blocks, no limit on size beyond `claims.max-width`.
- **Protected like any claim:** only players trusted there, or admins in bypass mode, can build.
  Use `/trust` while standing in it, or **Trusted players** in inspect.
- **Managed by admins:** anyone with `claimora.admin.modify` can grab its corners with the wand to
  resize it and make subclaims inside it, just like an owner. Every change is logged.
- **Never expire** with inactivity expiry, and don't count toward anyone's claims.
- **Teleport:** players can `/claim tp` to one only if they're trusted there.
- Deleted admin claims go to the recycle bin like any admin delete.
- WorldGuard's region check still applies, so give the region the `claimora-claim` flag if you want
  an admin claim inside it.

`/claimadmin claims server` lists them all.

## Recycle bin and restoring claims

Claims deleted **by an admin** or **by inactivity expiry** go to a recycle bin for
`admin.recycle-days` (default 30) instead of disappearing. Player abandons are *not* kept: the
player confirmed it and got their claim blocks back.

- `/claimadmin restore` lists the bin, newest first: name, ID, owner, why and when it was deleted.
  Tap one to restore it, or use `/claimadmin restore <id>`.
- A restored claim comes back with its **subclaims, trusted players, names and home points**, owned
  by the same player, with the same ID.
- If someone has claimed that land since, the restore is refused and names the claim in the way.
- A deleted **subclaim** can be restored only while its main claim still exists and it still fits
  inside it. If the main claim changed owner, the subclaim goes to the new owner.
- If the owner already has another claim with the same name, the restored one gets "Base 2" and so on.
- The restored land counts toward the owner's used claim blocks again, even if that takes their
  balance below zero.
- Entries older than `admin.recycle-days` are removed by the daily check. Set it to `0` to turn the
  bin off, and admin deletes then can't be undone (the confirmation says so).
- **Removing entries early, for good** (0.9.8): tap an entry in the bin and choose **Delete forever**, or use the
  buttons under the list (clear admin deletes, clear expired claims, empty the bin). From chat or the console:
  `/claimadmin restore purge <id>`, `purge admin`, `purge expired` or `purge all`. Each one first says what would be
  removed (with the count) and asks you to confirm: a confirm page in the menus, or the same command with
  `confirm` at the end, e.g. `/claimadmin restore purge admin confirm`. Removed entries can't be restored, and each
  removal is written to the admin log.
- Claim IDs are never reused, even after a bin entry is removed or a player abandons a claim, so the admin log never
  mixes up two claims with the same ID.

The delete confirmation shows the owner, size and how many subclaims go with it. If the claim changed
(new owner or size) while the dialog was open, it stops instead of deleting.

## The admin log

Admin actions are saved to the `admin_log` table in `claimora.db` and printed to the console:

```
[Claimora] [Admin] Koro deleted claim #12 'Base' of Steve
[Claimora] [Admin] Koro give 500 claim blocks for Steve
```

Logged: admin claim **creation** (and subclaims made inside admin claims), admin **teleports**, **trust/untrust**, **renames**, **transfers**, **resizes** (with the old
and new size), **deletes**, **restores** and **claim block** changes. Changes an owner makes to their
own claim aren't logged. Entries older than
`admin.log-days` (default 180) are removed once a day.

`/claimadmin log` shows the newest 50 entries, 10 per page: when, which admin, what they did, the
claim, its owner and the change (for example `24x30 at 100,-40 → 30x30 at 100,-40`).
`/claimadmin log Steve` narrows it to Steve's claims, `/claimadmin log #12` to one claim. Inspect
also has an **Admin log** button for that claim.

## Configuration

`plugins/Claimora/config.yml`. Change it, then run `/claimadmin reload`.

### Top-level switches

| Key | Default | Meaning |
| --- | --- | --- |
| `metrics` | `true` | Anonymous usage statistics through bStats (see [Usage statistics](#usage-statistics-bstats)). Needs a restart. |
| `update-checker` | `true` | Check Modrinth for a newer Claimora at startup and once a day (see [Update checker](#update-checker)). Needs a restart. |
| `startup-banner` | `true` | The Claimora logo and a short summary in the console at startup: version, server and Java version, storage and claims loaded, plugins Claimora hooks into, whether statistics are on, and the docs link. Handy to copy into a support request. `false` prints one line instead. Needs a restart. |

### `claim-blocks`

| Key | Default | Meaning |
| --- | --- | --- |
| `starting` | 100 | Blocks every player starts with. Changing it applies to everyone. |
| `per-active-hour` | 100 | Earned per hour of active play (moved in the last 5 minutes) |
| `max-earned` | 20000 | Cap on blocks earned from playtime. Admin grants are separate. |

### `claims`

| Key | Default | Meaning |
| --- | --- | --- |
| `min-width` | 5 | Smallest side length |
| `min-area` | 25 | Smallest area |
| `max-width` | 1000 | Largest side length |
| `auto-size` | 11 | Size of the square made by `/claim auto` |
| `naming-dialog` | true | Ask for a name after claiming |
| `enter-messages` | true | "Entering Base · Steve" on the action bar |
| `disabled-worlds` | [] | Worlds where claiming is off |

### `wand`

| Key | Default | Meaning |
| --- | --- | --- |
| `give-on-first-join` | true | Give the wand to new players |
| `reach` | 64 | How far the crosshair reaches while selecting |

### `visuals`

| Key | Default | Meaning |
| --- | --- | --- |
| `glowing` | true | Glowing outline, visible through walls |
| `view-radius` | 64 | Only border pieces this close are drawn |
| `max-segments` | 256 | Most border pieces shown to one player at once |
| `flash-seconds` | 6 | How long a border shows after a player is blocked |
| `beam-thickness` | 0.06 | Beam thickness, in blocks |
| `styles.<name>.block` / `.glow` | | Block and glow colour for `own`, `other`, `admin`, `subclaim`, `preview-valid`, `preview-invalid` |
| `bedrock.borders` | particles | `particles`, or `displays` (needs a Geyser add-on such as Scaffolding) |
| `bedrock.spacing` / `size` / `max-particles` | 1.0 / 1.0 / 600 | Particle settings |

Borders are **client-side only**. No real entities are spawned, so they can't lag the server or
be broken.

### `claim-options`

Which switches claim owners get in `/claim options` (or **Manage → Options** in `/claim menu`). Each is `true`
(owners can switch it per claim) or `false` (hidden; claims keep vanilla behaviour and owners' earlier choices are
ignored until you turn it back on). These **don't** turn plugin features on or off: to turn Claim Fly off for the
whole server, use `claim-fly.enabled: false` (its switch then disappears here too).

| Key | Default | Owners can… |
| --- | --- | --- |
| `claim-options.crop-trampling` | `true` | stop farmland trampling in their claim |
| `claim-options.hostile-spawns` | `true` | stop hostile mobs spawning naturally |
| `claim-options.pvp` | `true` | stop players hurting each other |
| `claim-options.claim-fly` | `true` | stop `/claimfly` in their claim |

Configs from before 0.9.5 that still have the old `options.enabled` list keep working exactly as before; switch to
`claim-options` whenever you like (delete the old `options:` section when you do).

What each option does when a player switches it:

- `crop-trampling`: nobody tramples farmland in the claim, the owner and trusted players included.
- `hostile-spawns`: hostile mobs don't spawn **naturally** (also patrols and zombie reinforcements).
  Spawners, spawn eggs, commands and plugin spawns are not affected, so mob farms and custom-mob
  plugins keep working.
- `claim-fly`: nobody can use Claim Fly in the claim (owner included). Hidden while `claim-fly.enabled` is
  `false`.
- `pvp`: players can't hurt each other while either of them is in the claim (melee, projectiles,
  harmful splash and lingering potions). It can only turn PvP **off**, never on where your world has
  PvP disabled. If you worry about players escaping fights into claims, use a combat plugin that stops
  tagged players entering claims, or remove `pvp` here.

Subclaims can follow their main claim (the default) or set an option themselves. A subclaim can
turn PvP back on inside a PvP-off claim, but never on where the world itself has PvP disabled.

Admins with `claimora.admin.modify` can change options on any claim or subclaim (inspect →
**Options**), and it's logged. Admin claims' options can only be changed by such admins.

### `protection`

| Key | Default | Meaning |
| --- | --- | --- |
| `public-blocks` | crafting_table, ender_chest | Blocks anyone may use inside claims |
| `allow-villager-trading` | true | Untrusted players may trade inside claims |
| `block-explosions` | true | Explosions don't break blocks inside claims |
| `block-fire-spread` | true | Fire doesn't spread or burn inside claims |

### `sounds`

`denied`, `trust`, `untrust`, `banned`, `teleport-tick`, `teleport-arrive` and `teleport-cancel`, each with
`sound` (a Minecraft sound id, or `none`), `volume` and `pitch`.

### `teleport`

| Key | Default | Meaning |
| --- | --- | --- |
| `enabled` | true | Allow `/claim tp` and the Teleport buttons. Off: `/claim tp` and `/claim sethome` say it's disabled, and the Teleport and **Set home here** buttons are hidden (a home only sets where `/claim tp` lands); admin teleport and `/claim trapped` still work. |
| `warmup-seconds` | 5 | Stand still this long (0 = instant). Moving or damage cancels. |
| `cooldown-seconds` | 30 | Time between teleports |
| `trusted-can-teleport` | true | Players may teleport to claims they're trusted in |
| `min-trust` | guest | Lowest trust level that may teleport to a trusted claim |
| `bar.length`, `filled-char`, `filled-color`, `empty-char`, `empty-color`, `format` | dots | The warm-up bar. `format` placeholders: `<claim>`, `<bar>`, `<time>`, `<seconds>`. |

### `claim-ban`

| Key | Default | Meaning |
| --- | --- | --- |
| `enabled` | true | Claim bans on or off (see [Claim bans](#claim-bans)) |
| `max-per-claim` | 50 | Most bans one claim can hold (1 to 500) |
| `message-cooldown-seconds` | 3 | Time between "You're banned" messages while walking into the border |

### `worldguard`, `expiry`, `admin`

| Key | Default | Meaning |
| --- | --- | --- |
| `worldguard.block-claims-in-regions` | true | See [WorldGuard](#worldguard) |
| `expiry.inactive-days` | 0 | See [Inactive-claim expiry](#inactive-claim-expiry) |
| `expiry.exempt` | [] | Names or UUIDs whose claims never expire |
| `admin.log-days` | 180 | Keep admin log entries this many days (0 = forever) |
| `admin.recycle-days` | 30 | Keep admin-deleted and expired claims restorable this many days (0 = no bin) |

## Messages

Every message is in `messages.yml`, written in
[MiniMessage](https://docs.advntr.dev/minimessage/format.html) (`<red>`, `<bold>`,
`<gradient:#55FFFF:#55FF55>`…). Words in angle brackets that aren't colours, like `<name>` or
`<id>`, are placeholders filled in by the plugin. Keep them. Missing keys fall back to the defaults.

## Bedrock (Geyser) support

- Bedrock players are detected through Floodgate or Geyser.
- Menus and the name box show as Bedrock forms. Light text colours are darkened automatically so
  they're readable on the light form background.
- Borders: `visuals.bedrock.borders: particles` (works everywhere) or `displays` (the same 3D beams
  as Java; needs an add-on like Scaffolding).

## Bedrock-style menus (Claimora-BedrockMenus)

Optional. Claimora's own menus work everywhere. For a Bedrock (Ore UI) look on Java, install two more jars. Both
come in the add-on's download (a zip with the two jars and short install steps):

| Jar | What it is |
| --- | --- |
| `Claimora-BedrockMenus-<version>.jar` | The add-on with every Bedrock-style screen. Released together with Claimora; use the **same version** as your Claimora. |
| `KoroUI-<version>.jar` | The engine that draws the screens, included in the add-on's download (it was called OracleUI before). If another plugin's download also includes KoroUI, keep only the newest KoroUI jar. |

- Players need KoroUI's resource pack. **With Nexo** (KoroUI 0.3.1 or newer) it's added to Nexo's pack automatically
  on every Nexo pack build; nothing to do. Without Nexo, add `plugins/KoroUI/KoroUI-pack.zip` to your server's
  resource pack. Players without the pack, and Bedrock players, keep the normal menus.
- What's covered: the main menu (banner, blocks left, Get claim wand), Your claims and each claim's page (info,
  Teleport, Set home here, Rename, option switches, Trusted players, Banned players, Subclaims, Transfer,
  Abandon), subclaims and their pages, the "Teleport to" chooser, Trusted players (add, levels, remove), Trusted
  claims, Buy blocks (step slider), Help, the claim ban screens, `/claim options` and `/claim tp` with several
  matches, and the "Name your claim" prompt after creating a claim.
- **Admin screens** (since 0.9.6, needs **KoroUI 0.3.2** or newer): `/claimadmin claims <player>` and `claims server`,
  the admin claim page from `/claimadmin inspect` (info with **Teleport**, Trusted players, Subclaims, Rename, the
  option switches, Admin log, the owner's claims, then Resize, Transfer and red Delete; a yellow **ADMIN** tag in
  the header), Transfer with its confirm page, the delete confirm, the recycle bin (`/claimadmin restore`, tap a
  claim to get a restore confirm) and the admin log. Every button checks the admin permission again when it's
  clicked (`claimora.admin.view`, `modify`, `teleport`, `delete`), so a menu opened before a demotion can't be used
  afterwards. With an older KoroUI, Bedrock admins, or admins without the pack, the normal admin menus open.
- **Long lists get a pager** at the bottom (`< Previous`, "Page 1 of 2", `Next >`): 20 rows per page for players,
  24 for claims and recycle-bin entries, 12 for subclaims, 20 for admin-log entries. Each click rebuilds the page
  from live data. (Before 0.9.6 long lists fell back to the normal menus.)
- Texts are the `ore-*` keys in Claimora's `messages.yml` (plain text; the theme sets colours and fonts).
- If KoroUI or the license key is missing, or the add-on's version doesn't match Claimora's, the add-on says so in the console and
  switches itself off; Claimora keeps its normal menus. Removing either jar does the same. Nothing is stored per
  player, and `/claimadmin memory` shows whether a menu add-on is installed.
- Option switches save straight away; repeated clicks on the same claim are merged into one database write, and
  KoroUI ignores clicks closer than 150 ms apart.

### License key

Claimora-BedrockMenus is a paid add-on and needs a license key. You get your key in your purchase ticket on our
Discord; it looks like `KORO-XXXX-XXXX-XXXX-XXXX`.

1. Start the server once with the add-on installed. It creates `plugins/Claimora-BedrockMenus/config.yml`.
2. Paste your key there: `license-key: "KORO-XXXX-XXXX-XXXX-XXXX"`.
3. Restart. The console says `License OK: Bedrock-style menus are on.`

How the check works:

- It runs in the background, once at startup and then about once a day. The server never waits for it.
- After the first successful check, restarts unlock the menus straight away from the saved result
  (`plugins/Claimora-BedrockMenus/license.dat`).
- If the license server can't be reached (your host blocks it, or it's down), the menus stay on for up to
  **10 days** after the last successful check, and the console says until when. After that they switch to the
  normal menus until a check works again; this happens by itself, no restart needed.
- Without a valid key, only the Bedrock-style menus are off: Claimora keeps working with its normal menus.
  Claims, trust, data and other plugins are never touched.

**Server IPs (whitelist):** each key works on up to **3 server IPs** (servers on the same machine share one IP).
The **first server that uses a new key is whitelisted automatically**; the console says
`This server's IP … was whitelisted for your key (1 of 3)`. To add or remove servers, use **`/mylicenses`** in the
Koro Labs Discord (➕ Add IP / ➖ Remove IP), or the 🖥️ Manage servers button under your key in your ticket. You can
change a key's IPs 3 times per 24 hours. A server whose IP isn't on the list switches to the normal menus and the
console names the exact IP to add; after you add it, the add-on picks it up within an hour, or at once after a
restart. Forgot your key? `/mylicenses` shows it (only to you).

**What is sent:** only your key, a random id for this server (made on first start; it says nothing about your
machine), a one-time random number and the add-on version, over HTTPS to `license.oraclecraft.xyz`. Like any
website, the license server sees the IP your server connects from; it stores only the IPs on your key's whitelist.
No player data, no hardware details, no server files. Don't copy `license.dat` between servers.

| Console message | What to do |
| --- | --- |
| `No license key set in …config.yml` | Paste your key into `license-key` and restart. |
| `license-key … doesn't look like a key` | Copy the key again exactly (including `KORO-`) and restart. |
| `This license key doesn't exist` | Check for typos (`/mylicenses` shows your key); if it still fails, ask in your ticket. |
| `This server's IP … isn't whitelisted for your license key` | Add that IP with `/mylicenses` (or remove an old one first if all 3 are used). |
| `This license key was revoked` / `has expired` | Contact us in your ticket. |
| `Couldn't check the license (…)` | Nothing yet: the menus stay on while the last check is valid. If it lasts, check that your host allows outgoing HTTPS to `license.oraclecraft.xyz`. |

**Updating from Claimora 0.8.x with OracleUI:** the Bedrock-style screens moved out of `Claimora.jar` into
`Claimora-BedrockMenus.jar`, and OracleUI is now KoroUI. Replace `OracleUI.jar` with `KoroUI.jar` (settings move
over by themselves) and add `Claimora-BedrockMenus.jar`. Without the add-on, Claimora 0.9.0 shows normal menus only.

## Normal menu additions (0.8.2)

- The normal `/claim menu` has a **Get claim wand** button (`menu-main-wand`), same as `/claim wand`.
- The normal Buy screen has Minecraft's own slider for any amount between `buy.min-amount` and what the
  player may still buy (`buy-slider-label`, `buy-slider-button`), followed by the `presets` buttons and the
  usual confirm step. Bedrock players see it as a form slider through Geyser.

## WorldGuard

With WorldGuard installed and `worldguard.block-claims-in-regions: true`, players can't claim land
that overlaps a WorldGuard region. To allow claiming in a region:

```
/rg flag <region> claimora-claim allow
```

## PlaceholderAPI

| Placeholder | Shows |
| --- | --- |
| `%claimora_blocks_available%` | Claim blocks the player can still spend |
| `%claimora_blocks_total%` | Total claim blocks |
| `%claimora_blocks_used%` | Blocks used by their claims |
| `%claimora_claims%` | Number of claims they own |
| `%claimora_claim_name%` | Name of the claim they're standing in ("Wilderness" outside) |
| `%claimora_claim_owner%` | Owner of the claim they're standing in |
| `%claimora_can_build%` | Whether they can build where they stand |
| `%claimora_fly_remaining%` | Claim Fly time left today (`42:17`, `unlimited`, or empty without a tier) |

## Inactive-claim expiry

Off by default. Set `expiry.inactive-days` to, for example, `60`, and once a day Claimora deletes
the claims of players who haven't joined for that long. Players in `expiry.exempt` are skipped,
and every removal is printed to the console. Expired claims go to the
[recycle bin](#recycle-bin-and-restoring-claims), so you can restore them for 30 days.

## Moving from GriefPrevention

Claimora can copy GriefPrevention's claims, trust and claim blocks. It reads GP's default file storage
(`plugins/GriefPreventionData/ClaimData/*.yml` and `PlayerData/`) and never changes those files.
GriefPrevention doesn't need to be running; the folder just has to be there. GP stored in MySQL isn't
supported. Add-ons such as GriefPrevention GUI keep the claims in GP's files, so they're covered; the
add-on's own extras (icons, flags) aren't imported.

1. Install Claimora next to the `GriefPreventionData` folder (you can keep or remove GP; remove it
   before players join so the two don't both protect the same land).
2. Run `/claimadmin import griefprevention` (console recommended). You get a **preview**: how many
   claims, subclaims, admin claims and players would be imported, and what would be skipped.
   Nothing changes yet.
3. Within 60 seconds run `/claimadmin import griefprevention confirm`. Claimora first backs up its
   database to `claimora-before-gp-import-<date>.db`, then imports in small batches per tick so the
   server doesn't freeze, and prints a summary.

**What maps to what:**

| GriefPrevention | Claimora |
| --- | --- |
| Top-level claim | Main claim (full height; GP's depth is dropped), named "Claim 1, 2…" per owner |
| Subdivision | Subclaim (nested ones from 3D forks are flattened into the top-level claim) |
| Admin claim | Admin claim |
| Accessors / Containers / Builders / Managers | Guest / Containers / Build / Manager (highest wins) |
| `public` trust | Not copied (counted in the preview) |
| Accrued / bonus claim blocks | Earned blocks / admin bonus (only raised, never lowered) |

**Skipped and reported:** claims overlapping an existing Claimora claim, claims in worlds that aren't
loaded, unreadable files, and subclaims that don't fit inside their claim or overlap another one.
Running the import again is safe: everything already imported overlaps itself and is skipped, and
claim blocks are only raised. Requires `claimora.admin`; logged in the admin log.

## Usage statistics (bStats)

Since 0.9.7 Claimora sends anonymous usage statistics to [bStats](https://bstats.org/plugin/bukkit/Claimora/35703),
like most public plugins: the number of servers and players, server software and version, Java version, country,
and whether the Bedrock-style menus add-on is installed. No player names, UUIDs or claim data. The charts are
public; what bStats collects and keeps is described at bstats.org.

- Turn it off for Claimora only: `metrics: false` in `config.yml`, then restart.
- Turn it off for every plugin: `enabled: false` in `plugins/bStats/config.yml`.

It sends a small report about every 30 minutes in the background; the server never waits for it.

## Update checker

Since 0.9.7 Claimora asks [Modrinth](https://modrinth.com/plugin/claimora) at startup and then once a day whether a
newer release is out. If there is one, the console says so once, and players with `claimora.admin.updates` (ops by
default) get a chat line with the download link when they join. Nothing is downloaded or installed by itself.

- The request only names Claimora and its version (Modrinth asks every program to say who it is). Nothing about
  your server or players is sent.
- It runs in the background; if Modrinth can't be reached, nothing happens and Claimora works the same.
- Turn it off with `update-checker: false` in `config.yml`, then restart.

## Data and backups

Everything is in `plugins/Claimora/claimora.db` (SQLite). Back it up with the server stopped, or
copy `claimora.db`, `claimora.db-wal` and `claimora.db-shm` together. Saving happens on a
background thread, so the server never waits on the database.

## Performance and memory

- Claims are kept in memory, grouped by chunk, so checking a block only looks at the claims in that
  chunk.
- Claim block balances are only kept for online players.
- Claimora runs just two repeating tasks: the wand (every 2 ticks, and only for players holding it)
  and a 5-minute task for playtime blocks, cleanup and the daily checks.
- Borders are packets, not entities.

`/claimadmin memory` lists everything Claimora keeps in memory. After players leave, the per-player
numbers should drop back to about the number of players online. If something keeps growing, please
report it with that output.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| Claimora doesn't enable | Install PacketEvents 2.14.0+ and check you're on Paper 26.1.2 to 26.3 with Java 25 |
| No borders on Bedrock | Use `bedrock.borders: particles`, or install Scaffolding for `displays` |
| Bedrock menu text hard to read | Update Claimora (0.3.6+ darkens colours automatically) |
| Players can't claim in a region | Set the `claimora-claim` flag, or turn off `worldguard.block-claims-in-regions` |
| Changed config doesn't apply | Run `/claimadmin reload` |
| "Could not open the Claimora database" | The database file is locked or damaged: stop the server and restore a backup |
