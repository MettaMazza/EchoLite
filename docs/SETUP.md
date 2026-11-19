# Install EchoLite

**Active development — Apple Silicon macOS and ARM64 Android 12 or newer.**

## Mac

Download or clone this repository, then open **Install EchoLite.app** at its root. The native installer downloads and verifies the pinned runtime, Qwen3.5-4B, vision and embedding models, Kokoro voice, dictionary and Wikipedia archive. It creates fresh private memory, registers background sync, adds EchoLite to your Applications folder and opens the chat interface. No compiler, Terminal commands, Python environment or manual asset placement is needed.

The installer and matching release assets must be published together. A maintainer's unpublished candidate cannot download assets from a release that has not been uploaded yet.

Allow approximately **60 GB of free disk space** and a large initial download (about **54 GiB**). Most of this is the complete offline English Wikipedia archive. Downloads resume after interruption and are SHA-256 checked before installation. Reopening the installer reuses verified cached files.

Installation: `~/Library/Application Support/EchoLite`

Download cache and setup log: `~/Library/Caches/EchoLite-install`

App shortcut: `~/Applications/EchoLite.app`

## Connected Android phone

Connect one phone by USB and open **Install EchoLite on Android.app**. It prepares the Mac if needed, installs the APK, transfers the offline resources, pairs the two devices and opens EchoLite. Existing conversations are retained when updating. Existing matching phone assets are reused; interrupted model transfers resume. Qwen3.5-4B is the default. An already installed 9B model remains an optional choice.

Android requires USB debugging and the phone owner's approval of the connected Mac. These OS security approvals cannot be silently bypassed. Allow approximately 60 GB free on a fresh phone. The current installer runs on Apple Silicon macOS; Windows/Linux installers are not included.

The APK includes its own adaptive app icon and launcher entry. On first launch it requests a home-screen shortcut on launchers that support pinning. Some launchers add it immediately; others require **Add to Home screen** confirmation. The app cannot override a launcher's placement policy.

Installing only the APK gives you the app and icon; the connected-phone installer also provisions the model/reference assets required for local inference.

## OS-controlled permissions

The development Mac installer is ad-hoc signed, not Apple-notarized. macOS may require you to approve opening it in Privacy & Security. Never disable Gatekeeper globally. Android may ask for notification permission and launcher confirmation. For reliable screen-off sync, allow EchoLite background running and automatic/secondary launch in your phone's battery settings; exact controls vary by manufacturer. Force-stop intentionally prevents Android restart until the app is opened again. Safari automation requires Safari's own remote-automation approval.

Web requests are attempted automatically, with local references and retained snapshots available when connectivity fails. There is no online/offline mode toggle.

## Development

The installer is authored in Ernos Plain. Its native dependency binaries and model/reference content retain their upstream licenses. See [third-party components](../THIRD_PARTY.md).

To modify/build the app itself, see [Build from source](BUILD-FROM-SOURCE.md) and [Android implementation](android/FOUNDATION.txt). Source compilation is separate from the ready-built installation path.
