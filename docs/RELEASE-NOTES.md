# EchoLite — active development

A local cognitive agent for Apple Silicon Macs and ARM64 Android phones,
authored in Ernos Plain. Qwen3.5-4B is the default model, with persistent
layered memory, editable Me, model-based recall, local references and tools.

This development update includes:

- Native Mac and connected-phone installers with verified, resumable downloads.
- A mint adaptive Android icon and first-launch home-screen pin request.
- Automatic paired USB/Wi-Fi background sync.
- Local Kokoro neural read-aloud and reply copy controls.
- Android text editing/selection improvements and a clean dark chat interface.

Open Install EchoLite.app on a supported Mac. To install the phone app,
connect an ARM64 Android 12+ phone and open Install EchoLite on Android.app.
The installers handle the model, reference and runtime assets; no compiler
or manual file provisioning is needed. First-time OS permissions still apply.
Full offline assets require approximately 60 GB free. Installing just the
APK supplies the app/icon; use the connected-phone installer to provision its
local models and references as well.

This is a prerelease under active development. The Mac installer is ad-hoc
signed rather than notarized. Device-specific battery policies can require
background-running approval. See the repository setup and validation notes.

## Next planned update

EchoOffGrid ecosystem integration: a dedicated tab to download, open and connect
EchoOffGrid, followed by tools to find, read, cite and open its library content.
The wider roadmap covers its reference, research, media, maps, download and
device systems. This is planned work, not part of the current release. EchoLite
remains independently usable. See integration plan.txt in the source repository.
