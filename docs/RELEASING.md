# Preparing a development release

Keep **active development** visible in the README, setup UI and release description. Do not publish personal state, device identifiers, pairing keys, signing keys, downloaded model files or private Git history.

The native EP `src/release_builder.ep` entry point accepts:

```text
CHECKOUT ANDROID_WORK EMPTY_OUTPUT_DIRECTORY RELEASE_TAG
```

Build the EP setup launcher and worker first, then the Android APK. The builder compiles the Mac application and library helper, packages explicit runtime/support allowlists, records SHA-256 digests, and creates two native installer apps. It uses the provisioned development dependencies; it is a maintainer tool, not a prerequisite for end users.

The installer apps must contain the exact final `release.json`, final setup-worker binary and dependency notices, then be signed. Repackaging a runtime or APK requires refreshing its manifest digest and both installer bundles. Keep the APK signing identity stable across updates; never commit its private key.

Publish together:

- `Install EchoLite.zip`
- `Install EchoLite on Android.zip`
- `EchoLite-Android-arm64.apk`
- `echolite-macos-arm64.tar.gz`
- `echolite-offline-support.tar.gz`
- `echolite-usb-tools-macos.tar.gz`
- `release.json` and `SHA256SUMS`
- Applicable upstream notices and corresponding dependency sources with the binary distribution.

Keep the two `.app` bootstrap bundles at the public repository root so a clone or GitHub source ZIP has a graphical entry point. The large assets belong in the matching GitHub Release, not Git history. Publish that matching release with the source update; otherwise the installers will report unavailable release downloads. Use a prerelease, not a stable release, for this development candidate.

After publication, verify the public release URLs and download/install path once through GitHub. Local-mirror validation does not test GitHub upload permissions or release visibility.
