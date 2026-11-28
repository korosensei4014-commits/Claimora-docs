# Developer API

[← Back to the wiki](README.md)

## Getting the API

The API is published from the public [Claimora-API](https://github.com/korosensei4014-commits/Claimora-API)
repository (API only, versions match Claimora's):

```kotlin
repositories {
    maven("https://raw.githubusercontent.com/korosensei4014-commits/Claimora-API/repo/")
}
dependencies {
    compileOnly("com.korosensei.claimora:claimora-api:0.8.6")
}
```

Always `compileOnly`: Claimora provides the classes at runtime.

Claimora registers its API with Bukkit's ServicesManager. Add `softdepend: [Claimora]` (or
`depend`) to your `plugin.yml`, then:

```java
ClaimoraApi api = ClaimoraApi.get(); // null if Claimora isn't enabled
if (api != null) {
    api.claimAt(location).ifPresent(claim -> getLogger().info("Owned by " + claim.owner()));
}
```

## `ClaimoraApi`

| Method | Returns |
| --- | --- |
| `claimAt(Location)` | The innermost claim there (a subclaim if there is one) |
| `canBuild(Player, Location)` | Whether the player may build there |
| `claimsOf(UUID owner)` | That player's main claims |
| `claimById(long id)` | A claim by ID |
| `giveClaimBlocks(UUID, long)` | Adds blocks (works offline). Completes with the new available amount. |
| `availableBlocks(UUID)` | Available blocks, for online players only (empty when offline) |
| `isBanned(long claimId, UUID)` | Whether the player is banned from that claim (a subclaim answers for its main claim). Since 0.8.6. |

## `ClaimInfo` (read-only)

`id()`, `worldId()`, `minX()`, `minZ()`, `maxX()`, `maxZ()`, `area()`, `owner()` (null for admin
(server) claims and subclaims inside them), `trustLevel(uuid)` (a `TrustLevel`: `GUEST`, `CONTAINERS`,
`BUILD`, `MANAGER`, or null if not trusted), `name()`, `trusted()`, `isSubclaim()`, `parentId()`, `contains(x, z)`.

## Events

All are cancellable and fire on the main thread.

| Event | When |
| --- | --- |
| `ClaimCreateEvent` | A claim or subclaim is about to be created |
| `ClaimResizeEvent` | A claim is about to change size |
| `ClaimDeleteEvent` | A claim is about to be deleted. `getReason()`: `ABANDONED`, `ADMIN`, `EXPIRED`, `PLUGIN` |
| `ClaimTrustEvent` | A player is about to be trusted, untrusted or moved to another level. `getLevel()` is the level given (or removed). |
| `ClaimTeleportEvent` | A player is about to start a `/claim tp` warm-up |
| `ClaimTransferEvent` | A claim is about to change owner |
| `ClaimBanEvent` | A player is about to be banned from (`isBanning()` true) or unbanned from a main claim. Banning also removes their trust there. Since 0.8.6. |

Admin claims have no owner: `ClaimCreateEvent#getOwner()` and `ClaimTransferEvent#getOldOwner()` /
`getNewOwner()` return null for them.

Restoring a claim from the admin recycle bin (`/claimadmin restore`) doesn't fire
`ClaimCreateEvent`: the claim already existed and keeps its ID.

Don't keep `ClaimInfo` objects for long: they're snapshots. Keep the ID and look the claim up again.
