# Versioning

Radixel OTA uses **path + manifest** versioning:

| Layer | Value |
| ----- | ----- |
| Folder | `android/<expo.version>/` or `ios/<expo.version>/` |
| Manifest `runtimeVersion` | Same string as `expo.version` in the app |
| Binary `updates.url` | `https://ax108.github.io/radixel-ota/{platform}/{expo.version}/manifest.json` |

## Rules

1. **JS-only OTA** — keep `expo.version` unchanged; overwrite that version’s folder only.
2. **Native / SDK change** — bump `expo.version`, ship a new store binary, publish a **new** version folder. Leave old folders in place.
3. Never point a binary at a version folder whose `runtimeVersion` does not match.
4. Never publish a broken/placeholder `manifest.json` (missing or invalid bundle URLs).

App: https://github.com/Ax108/radixel  
CDN: https://github.com/Ax108/radixel-ota
