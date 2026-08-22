# radixel-ota

Self-hosted **Expo Updates** CDN for [Ax108/radixel](https://github.com/Ax108/radixel).

| | |
| --- | --- |
| Host | **GitHub Pages** |
| Layout | Versioned `{platform}/{appVersion}/` |
| Protocol | Expo Updates **v1** (`manifest.json` + `bundles/` + `assets/`) |
| Not used | EAS Update |

Public base (after Pages is enabled):

```text
https://ax108.github.io/radixel-ota/
```

## Path contract

```text
android/<expo.version>/manifest.json
android/<expo.version>/bundles/…
android/<expo.version>/assets/…

ios/<expo.version>/manifest.json
ios/<expo.version>/bundles/…
ios/<expo.version>/assets/…
```

Example for app version `1.0.0`:

```text
https://ax108.github.io/radixel-ota/android/1.0.0/manifest.json
https://ax108.github.io/radixel-ota/ios/1.0.0/manifest.json
```

`runtimeVersion` inside each manifest **must** equal that folder’s `<expo.version>` (`app.json` → `runtimeVersion.policy: "appVersion"`).

Old version folders stay on the CDN so older binaries keep working. Publish **only replaces** the folder for the version you export.

## Repo layout

```
radixel-ota/
├── index.html                      ← tiny landing page
├── README.md                       ← this file
├── VERSIONING.md
├── .github/workflows/static.yml    ← Pages deploy on `main`
├── android/
│   └── <appVersion>/               ← created by app publish script
│       ├── manifest.json
│       ├── bundles/
│       └── assets/
└── ios/
    └── <appVersion>/
        ├── …
```

Do **not** commit placeholder manifests with fake bundle URLs — clients may try to download them. Empty platform folders are fine until the first real publish from the app.

## One-time: enable GitHub Pages

1. Repo **Settings → Pages → Source: GitHub Actions**.
2. Push **`main`** (this workflow’s trigger).
3. Wait for the **Deploy static content to Pages** Action.
4. Confirm `https://ax108.github.io/radixel-ota/` loads.

## Publish flow

From [Ax108/radixel](https://github.com/Ax108/radixel):

1. Confirm `expo.version` matches the binary’s `runtimeVersion`.
2. Run `bun run ota:export:android` and/or `bun run ota:export:ios` (stages into this repo).
3. Commit + push **this** CDN repo’s **`main`** branch so Pages deploys.
4. Cold-start a matching native build — it checks the baked `updates.url` for that platform/version.

## Related

- App: https://github.com/Ax108/radixel
