# Key entry points

For users: open the app → server list or TV home. This page is only a maintainer map.

| Piece | Role |
|-------|------|
| Application class | Loads the server fleet, wires notifications / wake lock |
| Main activity | Phone Compose UI or TV shell; permissions; ads (Free) |
| Foreground service | Keeps servers reachable with an ongoing notification |
| Boot receiver | Optional start-after-boot when the user enabled it |

Lifecycle detail: [../architecture/runtime-lifecycle.md](../architecture/runtime-lifecycle.md).
