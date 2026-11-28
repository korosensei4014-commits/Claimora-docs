# Changelog

[← Back to the wiki](README.md)

## 1.0.0

First public release.

- **Runs on Paper 26.1.2 and 26.2 as well as 26.3** (and forks), from the same jar. Before, Claimora failed to start
  on 26.1.2. Every release is now started on all three versions before it's published.
- The Bedrock-style menus add-on runs on all three too, with **KoroUI 0.3.3** (older KoroUI only loads on 26.3).
- No gameplay, config, database or command changes.

## 0.9.8

- **Remove recycle-bin entries for good:** tap an entry and choose **Delete forever**, or clear all admin deletes,
  all inactivity removals or the whole bin with the buttons under the list. Commands:
  `/claimadmin restore purge <id|admin|expired|all>`, which asks first (add `confirm` to go ahead). Needs
  `claimora.admin.delete`; each removal goes into the admin log. Works in the normal and the Bedrock-style menus.
- **Claim IDs are never reused** after a bin entry is removed or a claim is abandoned (before, a restart could
  hand an old ID out again and mix two claims up in the admin log). The database updates itself to schema 11 on
  first start (a new table; nothing existing changes).
- The recycle bin menu now remembers its page when you go to the next page.
- The Bedrock-style menus add-on download now includes KoroUI.

## 0.9.7

- **Anonymous usage statistics with bStats** ([Claimora on bStats](https://bstats.org/plugin/bukkit/Claimora/35703)):
  server and player counts, versions, and whether the Bedrock-style menus add-on is installed. Turn it off with
  `metrics: false` in `config.yml` (restart) or for every plugin in `plugins/bStats/config.yml`. See "Usage
  statistics" in the server owners guide.
- **Update checker:** Claimora checks Modrinth at startup and once a day for a newer release; the console says so
  and admins (`claimora.admin.updates`, ops by default) see a chat line on join. Off with `update-checker: false`.
- **Clear message on unsupported servers:** on plain Spigot or Folia, Claimora now says it needs Paper (or a Paper
  fork) and switches itself off, instead of failing with an error.
- **Startup banner:** the console shows the Claimora logo with the version, server and Java version, storage
  and claims loaded, the plugins Claimora hooks into, whether statistics are on, and the docs link. Turn it off
  with `startup-banner: false` in `config.yml`.
- No gameplay, database, permission or command changes.

## 0.9.6

- **Claimora-BedrockMenus: admin screens in the Bedrock style** (needs KoroUI 0.3.2): a player's claims, server
  claims, the admin claim page (one **Teleport** button, Trusted players, Subclaims, Rename, option switches, Admin
  log, Resize, Transfer, Delete, with a yellow ADMIN tag), Transfer and Delete confirm pages, the recycle bin with a
  restore confirm, and the admin log. Every button re-checks the admin permission when clicked. Bedrock admins,
  admins without the pack and servers with an older KoroUI keep the normal admin menus.
- **Claimora-BedrockMenus: long lists page instead of switching to the normal menu.** Your claims, Trusted claims,
  subclaims, Trusted players, Add player, Banned players, Ban player, Transfer, `/claim options` and `/claim tp`
  lists show `< Previous` / `Next >` at the bottom when they don't fit on one page.
- New `ore-admin-*` and `ore-pager*` texts in `messages.yml` (old files get them automatically).
- No database, permission or command changes. Claimora's normal menus are unchanged.

## 0.9.5

- **Clearer claim options in `config.yml`:** the new `claim-options` section has one `true`/`false` per switch
  owners get in `/claim options`, with comments saying it doesn't turn features on or off. Old configs with
  `options.enabled` keep working unchanged. The Claim Fly switch now hides itself while `claim-fly.enabled` is
  `false`, and the `claim-fly` section says plainly how to turn the feature off.
- **With `teleport.enabled: false`,** the **Set home here** button is hidden too (normal and Bedrock-style menus),
  and `/claim sethome` says teleporting is disabled. A home only sets where `/claim tp` lands.

## 0.9.4

- **Claimora-BedrockMenus: license keys now use a server IP whitelist** (up to 3 IPs per key). The first server
  that uses a new key is whitelisted automatically; more servers are added with `/mylicenses` in the Koro Labs
  Discord. A server whose IP isn't whitelisted gets the normal menus and the console names the IP to add; the add-on
  re-checks hourly, so no restart is needed after adding it. `/mylicenses` also shows your key if you forget it.
- Claimora itself is unchanged for players. No database, permission or command changes.

## 0.9.3

- **Claimora-BedrockMenus now needs a license key** (`license-key` in `plugins/Claimora-BedrockMenus/config.yml`;
  see "License key" in the server owners guide). The key is checked in the background at startup and about once
  a day; the server never waits. A successful check keeps working for up to 10 days if the license server can't
  be reached. Without a valid key only the Bedrock-style menus are off and Claimora shows its normal menus.
- Claimora itself is unchanged for players. No database, permission or command changes.

## 0.9.2

- The Claimora-BedrockMenus jar is now obfuscated (its internal class names are scrambled). Nothing changes in
  game and there's no speed cost. Claimora itself is unchanged.

## 0.9.1

- Claimora-BedrockMenus: the **"Name your claim"** prompt after creating a claim is now a Bedrock-style page too
  (same text box and Save as Rename; closing it keeps the default name). Bedrock players and players without the
  pack keep the normal dialog with Save and Skip. New `messages.yml` keys `ore-name-new-*`.

## 0.9.0

- **The Bedrock-style (Ore UI) menus are now a separate add-on, `Claimora-BedrockMenus`,** released together
  with Claimora (same version, both jars on the release). `Claimora.jar` alone has the normal menus only, on
  Java and Bedrock alike; nothing else in Claimora changes.
- **OracleUI is now called KoroUI.** To keep the Bedrock-style menus: replace `OracleUI.jar` with `KoroUI.jar`
  (its settings move over by themselves) and add `Claimora-BedrockMenus-0.9.0.jar`.
- The add-on checks that Claimora has the same version and that KoroUI is running; otherwise it says why in the
  console and switches itself off, leaving Claimora on its normal menus.
- `/claimadmin memory`: "Ore UI menus (OracleUI)" is now "Menu add-on".
- No database, config, permission or command changes. `messages.yml` keeps every `ore-*` text, so your edits stay.

## 0.8.8

- Fixed: a banned player riding into a claim got the "You were moved out" message twice.

## 0.8.7

- Fixed: a banned player could still ride into the claim on a horse, pig or other mob. Riders are now checked
  every 2 ticks: a banned rider is dismounted, moved just outside, and the mount is brought along.
- Fixed: long grey info lines at the top of Ore UI pages (for example "Ban a player" and "Subclaims") were cut
  off with "...". They now wrap onto more lines.

## 0.8.6

- **Claim bans.** Owners can ban a player from a claim and its subclaims with `/claim ban <player>` (no name:
  pick from a menu), `/claim unban <player>` and `/claim bans`, or from the new **Banned players** row on the
  claim's page (normal menus and Ore UI). A banned player is stopped at the border with a message, a sound and
  a short border flash, can't pearl, ride or teleport in, and is moved just outside if they're inside. Banning
  removes their trust in that claim and its subclaims, and banned players can't be trusted until unbanned.
  New permissions `claimora.ban` (everyone) and `claimora.ban.bypass` (op), config section `claim-ban`, sound
  `sounds.banned`, and many new `messages.yml` keys (`ban-*`, `unban-*`, `ore-ban*`, `ore-bans-*`,
  `help-bans`, `ore-help-bans`). The database moves to schema 10 (a new `claim_bans` table) automatically.
- **Claim teleport can be turned off:** `teleport.enabled: false` disables `/claim tp` and hides the Teleport
  buttons in the menus. Admin teleport and `/claim trapped` still work. New key `tp-disabled`.
- Developer API: `ClaimBanEvent` and `ClaimoraApi#isBanned(claimId, player)`.
- `/claimadmin memory` shows the number of stored bans and ban notices.

## 0.8.5

- **`/claimadmin blocks set <player> <n>` now sets the blocks the player has left to spend.** Their claims
  keep the blocks they use, so `set 0` leaves them at 0 (it used to set the total and could give -250).
- Ore UI main menu: the banner now says **"711 blocks left"**, with "of 929" (your total) still on the right
  (new keys `ore-blocks-left`, `ore-blocks-left-one` and `ore-blocks-right`). It never shows a negative number.
- Ore UI **Your claims** list (it still opened the normal list), the `/claim options` "pick a claim" list
  outside a claim, and `/claim rename` without a name now use Ore UI too. New keys `ore-own-*` and
  `ore-options-pick-*`.

## 0.8.4

- **More Ore UI** (with OracleUI 0.2.4 or newer): **Rename** (current name, Minecraft's own text box and a
  Save button under the page), **Transfer** (pick an online player, then confirm) and the **Abandon**
  confirmation.
- `/claim options` opens the claim's Ore UI page (its option switches are there) for the owner; admins and
  players without Ore UI keep the normal Options menu.
- `/claim tp <name>` with several matches shows the Ore UI list to pick from.
- Fixed: the Buy blocks page showed a negative balance ("-$7,900 left") when you couldn't afford the amount;
  it now says how much more you need (`ore-buy-after-poor`).
- New `messages.yml` keys `ore-cancel`, `ore-rename-*`, `ore-transfer-*`, `ore-abandon*` and `ore-pick-*`.

## 0.8.3

- Fixed: the `claimora.admin.modify` permission in `plugin.yml` was malformed (a misplaced block). Paper
  ignored the broken part, but stricter Paper forks refused to load Claimora. `claimora.admin.modify` now
  includes `claimora.admin.view` again, as documented.
- Ore UI Buy blocks now also shows your current claim blocks, the price per block and your balance after
  buying (new keys `ore-buy-balance-desc`, changed `ore-buy-after`).
- Developer API: the API classes are now published for other plugins (see the developer docs).

## 0.8.2

- **Ore UI everywhere in `/claim menu`** (needs OracleUI 0.2.2 or newer): Trusted players (add from online
  players, Guest/Containers/Build/Manager choice saved instantly, remove), Trusted claims and their pages,
  Buy blocks with a step slider, and Help.
- **Normal menu:** new **Get claim wand** button, and a slider for any amount on the Buy screen (the pack
  buttons stay).
- New `messages.yml` keys `ore-trust-*`, `ore-level-*`, `ore-shared-*`, `ore-buy-*`, `ore-help`,
  `ore-help-title`, `menu-main-wand`, `buy-slider-label` and `buy-slider-button` (defaults apply automatically).

## 0.8.1

- **Ore UI subclaims** (needs OracleUI 0.2.1 or newer): the Subclaims list, each subclaim's page
  (Teleport, Set home here, Rename, option switches, Trusted here, Abandon subclaim) and the "Teleport to"
  chooser for claims with subclaims. Without OracleUI nothing changes.
- `/claimadmin memory`: the "Ore UI main menu" line is now "Ore UI menus".
- New `messages.yml` keys `ore-subclaim*`, `ore-subclaims-*` and `ore-tp-*` (defaults apply automatically).

## 0.8.0

- **Ore UI claim page** (needs OracleUI 0.2.0 or newer): opening one of your claims from `/claim menu`
  shows a Bedrock-style page with Teleport, Set home here, Rename, instant option switches, Trusted
  players, Subclaims, Transfer and Abandon. Without OracleUI, or with an older one, nothing changes.
- Claim option changes are now saved as small option-only writes, and several quick changes to the same
  claim are merged into one. `/claimadmin memory` shows them on the "Database queue" line.
- New `messages.yml` keys `ore-claim-*` and `ore-option-*` (defaults apply automatically).

## 0.7.1

- Ore UI main menu: says "1 claim" and "1 block" instead of "1 claims" / "1 blocks". New `messages.yml` keys
  `ore-row-claims-desc-one` and `ore-blocks-one` (defaults apply automatically).
- Works best with OracleUI 0.1.3 or newer (window frame, banner edge, bigger arrows).

## 0.7.0

- **Ore UI main menu (optional):** with the OracleUI plugin installed, `/claim menu` shows an Ore UI-style
  main screen to Java players whose resource pack loaded: banner with available/total claim blocks and a
  progress bar, badge rows, and a **Get claim wand** button (same as `/claim wand`). Everyone else, and
  servers without OracleUI, keep the normal menu. New `ore-*` keys in `messages.yml` (defaults apply
  automatically). `/claimadmin memory` shows whether the Ore UI menu is active.

## 0.6.4

- **GriefPrevention import:** `/claimadmin import griefprevention` shows a preview;
  `... confirm` backs up `claimora.db` and imports claims, subclaims, admin claims, trust (as levels)
  and claim blocks from GP's files, in batches. Overlaps and problems are skipped and reported.
- `/claim buy` dialogs show your money, the price, and your money and claim blocks after buying.

## 0.6.3

- **Buying claim blocks:** `/claim buy [amount]` and **Buy claim blocks** in `/claim menu`, at
  `price-per-block` (2.0), with a confirmation. Needs Vault and an economy plugin (optional soft
  dependency). Limits: `min-amount`, `max-per-purchase`, lifetime `max-bought`. No selling back.
- Database schema 9 (adds `bought` to players, automatic).

## 0.6.2

- **Claim limits per rank:** `claimora.claims.max.<n>` (highest wins; none = unlimited). Applies to new
  claims, `/claim auto` and receiving transfers.
- **`/claim trapped`:** 10 s stand-still, then to the nearest safe spot outside the claim; 1 hour
  cooldown (`trapped` section in `config.yml`, `claimora.trapped.nocooldown`).

## 0.6.1

- Trusting a player now shows a message for their level (Guest, Containers, Build, Manager), for both
  you and them. These are new keys in `messages.yml` (`trusted-guest`, `trusted-you-guest` and so on),
  so they work without regenerating the file. The old `trusted` and `trusted-you` keys are no longer used.

## 0.6.0

- **Trust levels:** `/trust <player> [guest|containers|build|manager]`. Guest uses doors and buttons;
  Containers adds storage, animals and farming; Build adds breaking and placing (plain `/trust`);
  Manager can also trust others up to Build.
- Trust menus pick a level for each player; `/trustlist` shows levels and lets you change them.
- `teleport.min-trust` and `claim-fly.min-trust` (default `guest`).
- Existing trusted players become Build (no change in what they can do).
- API: `ClaimInfo#trustLevel`, `TrustLevel`, `ClaimTrustEvent#getLevel`.
- Database schema 8 (adds `level` to trust, automatic).

## 0.5.4

- **Daily Claim Fly time** per tier (`claim-fly.tiers`). Only real flying counts. Resets daily at
  `reset-time` in `timezone` (default 00:00 Asia/Manila), with no reset task and safe across restarts.
- Action bar shows time left; warnings at 5 and 1 minutes (`warn-at`); running out mid-air gives the
  grace countdown and a safe fall.
- **Claim Fly option:** owners can block Claim Fly in a claim or subclaim (`/claim options`). Existing
  servers: add `- claim-fly` to `options.enabled`.
- `/claimfly info` shows time left and the next reset.
- `/claimadmin flytime <player> [add|set|reset] [time]` (logged).
- `%claimora_fly_remaining%` placeholder.
- Database schema 7 (adds `fly_used` and `fly_period` to players, automatic).

## 0.5.3

- **Claim Fly:** `/claimfly` (or `/claim fly`) lets players with a `claimora.fly.tier.<name>`
  permission fly inside claims they own or are trusted in. `/claimfly info` shows the status.
- 3-second grace countdown after flying out of a claim, then a damage-free fall.
- PvP turns Claim Fly off and locks it for 10 seconds (`disable-on-pvp`).
- "✈ Claim Fly" action bar while flying.
- Never touches flight from game modes, `/fly` or other plugins.
- New `claim-fly` section in `config.yml`. Every tier is unlimited for now; daily times arrive in 0.5.4.

## 0.5.2

- Subclaim options are simply **Allowed** or **Blocked**. A subclaim follows its main claim until you
  change it; switching it back to match the main claim makes it follow again.
- Crop trampling shows **Blocked** instead of "Blocked for everyone".

## 0.5.1

- **Subclaim options:** each option on a subclaim can follow the main claim (default), or be set to
  allowed or blocked just for that subclaim. Use `/claim options` inside the subclaim or **Manage →
  Options** on it.
- `/claim options` inside a subclaim now opens that subclaim's options.
- No database change: the choice is stored in the existing `options` column.

## 0.5.0

- **Claim options:** `/claim options` (or **Manage → Options** in `/claim menu`) lets owners block crop
  trampling for everyone, stop hostile mobs spawning naturally, and turn PvP off in their claim.
  Subclaims follow their main claim.
- `options.enabled` in `config.yml` chooses which options players may use.
- Admins with `claimora.admin.modify` can change any claim's options from inspect (logged).
- Database schema 6 (adds the `options` column, automatic).

## 0.4.4

- Fixed: subclaims inside admin claims weren't drawn for admins after they were created (only the
  selection preview showed). Admins with `claimora.admin.modify` now see them, like an owner would.

## 0.4.3

- **Admin claims:** server-owned land with orange borders that costs no claim blocks and never
  expires. Make one with `/claimadmin adminclaim` and the wand, or turn a claim into one with
  `/claimadmin transfer <id> server`.
- Admins with `claimora.admin.modify` resize admin claims and make subclaims in them directly with
  the wand. Changes are logged.
- `/claimadmin claims server` lists all admin claims.
- Claims without an owner now show "Server" (was "Admin").
- API: the owner in `ClaimCreateEvent` and `ClaimTransferEvent` can be null (admin claims).

## 0.4.2

- **Recycle bin:** claims deleted by admins or by inactivity expiry are kept for
  `admin.recycle-days` (30). Player abandons aren't kept.
- **`/claimadmin restore`** lists the bin; `restore <id>` brings a claim back with its subclaims,
  trusted players, names and home points. It's refused if the land was claimed since.
- **`/claimadmin delete [id]`** asks to confirm (owner, size, subclaims) and stops if the claim
  changed meanwhile. Inspect has a **Delete** button.
- **`/claimadmin log [player|#id]`**: the newest 50 admin actions, 10 per page. Inspect has an
  **Admin log** button.
- Restores are logged.
- Database schema 5 (adds the `deleted_claims` table, automatic).

## 0.4.1

- **Admin changes:** `/claimadmin trust`, `untrust`, `rename`, `transfer` (offline players allowed)
  and `resize` (arms your wand for that claim). All need `claimora.admin.modify`.
- Inspect has **Trusted players**, **Rename**, **Transfer** and **Resize** buttons for admins with
  the modify permission.
- Admin resize and transfer ignore the owner's claim blocks.
- Transfer asks to confirm, and stops if the claim changed owner in the meantime.
- Every admin change is logged with before and after values.
- `/claimadmin claims` tab completion suggests offline players who own claims.
- `/trust` and `/untrust` on someone else's claim now need `claimora.admin.modify` (included in
  `claimora.admin`, so ops are unaffected).

## 0.4.0

- **Admin lookup:** `/claimadmin claims <player>` shows anyone's claims (online or offline) in a
  menu, with blocks used and when they were last seen.
- **Inspect:** `/claimadmin inspect [id]` shows owner, world, corners, size, creation date, home
  point, trusted players and subclaims. Works from the console with an ID.
- **Admin teleport:** `/claimadmin tp <id> [center]`, with no warm-up or cooldown, using the same
  safe landing as `/claim tp`.
- **Permission levels:** `claimora.admin.view`, `.teleport`, `.modify`, `.delete`. `claimora.admin`
  still grants everything.
- **Admin log:** teleports, deletes and claim-block changes are saved to the database and printed
  to the console. Kept for `admin.log-days` (180).
- `/claimadmin info` is now `inspect` (the old name still works).
- Database schema 4 (adds the `admin_log` table, automatic).

## 0.3.6

- Bedrock forms: light text colours are darkened so they're readable.
- `visuals.bedrock.borders`: `particles` or `displays`.

## 0.3.5

- Teleport on a claim with subclaims asks whether to go to the main claim or a subclaim.

## 0.3.4

- `/claim` gives the wand; `/claim menu` opens Your claims / Trusted claims / Help.
- Manage claims from anywhere in the menu.

## 0.3.3

- Menus, subclaim teleport, trust menus, `/claim transfer`, inactive-claim expiry, PlaceholderAPI.

## 0.3.0 – 0.3.2

- Subclaims, claim names and `/claim rename`, `/claim tp` with warm-up and cooldown, WorldGuard hook,
  developer API, sounds.

## 0.2.x

- Resizing by grabbing corners, `/claim auto`, Bedrock particle borders.

## 0.1.x

- First release: claiming with the wand, claim blocks, protection, client-side borders, trust.
