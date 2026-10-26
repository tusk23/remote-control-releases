# remote-control — release assets

Public package host for the **remote-control** installable core.

The source lives in a **private** repository. This repo exists only so the
phone can download releases **anonymously**: GitHub answers `404` to an
unauthenticated request for a private repo's release assets, and the core's
update client is deliberately credential-free (no GitHub account, no token).

| Asset | Purpose |
|---|---|
| `app-android-release.apk` | The signed installable core |
| `manifest.json` | `version`, `changelog`, `sizeBytes`, `sha256`, `artifact`, `publishedAt` |

## Verifying before you install

Never install an APK from here without checking it against the manifest:

```bash
sha256sum app-android-release.apk
# must equal the "sha256" field of manifest.json
```

The core does this automatically before handing any update to the Android
installer, and refuses to install on a mismatch.

## Provenance

Every release is published by CI from the private repository's `release`
workflow, which builds and signs the APK in the same job that computes the
`sha256` and writes the manifest. Nothing outside that job can rewrite either.

Release tags are **immutable**: a tag is never republished, because the hash in
a manifest already delivered to a phone is only a guarantee while the bytes
behind the tag cannot change. To publish, bump `VERSION` in the source repo.
