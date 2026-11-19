# Installer validation — 19 November 2026

The installer, library registration helper, Android lifecycle classes and release builder are authored in Ernos Plain.

Completed checks:

- A separate, empty Mac installation directory received the real packaged runtime, dictionary, voice, Qwen3.5-4B, vision projector, MiniLM and Wikipedia archive. Large existing assets were APFS-cloned into the test download cache; archive downloads exercised curl through a local file mirror. No personal memory was copied.
- SHA-256 verification, library registration and fresh memory initialization completed. The installed core reported valid database integrity and foreign keys, zero memories and one initial identity version.
- The installed dictionary passed 16 offline lookup/cache checks. The installed Kokoro runtime generated a completed WAV and completion marker from the new installation path.
- The native setup window displayed successful completion. A separate empty-cache run displayed the disk-space guard.
- A partially downloaded runtime archive resumed from its existing byte offset and completed. An intentionally wrong checksum stopped installation before extraction.
- Known-answer SHA-256, persistent verification reuse, invalidation after mutation, atomic file publication, literal process arguments and resumed stdin streaming passed.
- The final Applications shortcut, user LaunchAgent registration and app-open steps completed against the existing Mac installation, preserving its private state.
- The connected-phone installer completed on the REDMAGIC: APK update, support archive transfer, reuse of matching large assets, device-specific Wikipedia registration, existing-pair preservation, USB route configuration and app launch.
- Android reports the background SyncService running and the EchoLite home shortcut pinned. The mint adaptive icon was visually inspected on the actual home screen.
- The upstream pinned generation, vision, embedding and Wikipedia download URLs returned HTTP 200. The new GitHub release URLs become available when the prepared release assets are published.
- Installer app signatures were checked locally (ad-hoc signing). These checks do not constitute Apple notarization.

This validation used the development Mac and connected Android phone. It separates fresh asset/state installation from the final launch test on an already configured machine. The phone had already approved USB debugging and background permissions. Those first-time OS approvals remain user-controlled on another device. The previously completed full-context model validation was not rerun for this packaging change.

## v0.2.1-dev.20261119 package check

The updated Mac installer completed into a separate fresh directory using the final manifest and a local release-asset mirror. It reused verified unchanged model/reference downloads, installed the new runtime, registered the full offline library and initialized fresh state. The installed app entrypoint reported database integrity and foreign keys OK, zero memories, zero pending turns and one identity version; Storage reported an empty generated cache. Background registration and app launch were suppressed for this isolated check to preserve the user's active installation.

Both final installer bundles passed strict signature verification and contain the exact final release manifest. All release-hosted manifest entries matched their artifact sizes and SHA-256 hashes. The APK is versionCode 2 / versionName 0.2.1-dev and retains the prior signing certificate. Its functional app build was tested on the REDMAGIC during acceleration, storage and left-menu validation; the release rebuild adds the version metadata. Published URLs are verified separately after upload. Full-context capacity testing was not repeated for packaging.
