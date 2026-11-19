# Development status and limits

**Active development / experimental release.** The application runs on Apple Silicon macOS and ARM64 Android. Tests and demo journeys are useful evidence, not a promise that every interaction on every machine is finished or reliable.

## What has been exercised

- Local model/tool conversations, calculator answers and local reference use.
- Embedding recall, prior-session continuity, corrections and explicit forgetting.
- Versioned Me edits, actual tokenizer limits and next-turn activation.
- Task state, schedules, interruption, process output recovery and deliberate retries.
- Native chat streaming, a thinking dropdown, image input and document adapters.
- Browser create/navigate/read/close and launching a local application.
- Backup/restore recovery and failure boundaries.
- Belief assessments and time-based invalidation of cached recall.

The last full deterministic offline regression before the latest editor work passed **35 suites**. Focused editor work added **20 native editor checks** and **27 font-layout checks**, including real clipboard round trips, Unicode byte boundaries, insertion/selection replacement, bounded undo/redo and visible selection rendering. These tests do not certify complete macOS accessibility or every physical input sequence. The interface had a hidden-window launch bug and lacked normal editing controls; those reports prompted fixes. The interface now uses a shared dark pixel design: Retina rendering, compact pixel icons, monospaced text, consistent borders and spacing across Chat, Tasks, Memory and Me. The redesign passed 20 editor checks and 25 font-layout checks with the new font (the layout count depends on wrapping). All four views and the 900 × 600 minimum window were rendered and visually inspected. Headings, bullets and fenced code blocks receive basic formatting; this is not a complete Markdown renderer. UI polish remains ongoing.

The public screenshot shows an actual local Qwen greeting, with the visible reply exactly matching the model output. Its preview is limited to that verified turn. Earlier calculator journeys returned `47 × 19 = 893` using tool receipts; those are separate checks, not claims about the greeting screenshot.

## Performance evidence has a scope

Development used an M1 Mac with **8 GB unified memory**. One recorded ordinary offline UI journey sampled peak simultaneous process RSS at approximately **3.019 GiB**. RSS is not a complete measure of GPU allocation, physical footprint or unrelated applications.

A retained full-context experiment processed **262,112 prompt tokens plus a 32-token output reserve**, including warm and disk-restart reuse. That older experiment used the earlier MLX harness; it is **not** a fresh native llama.cpp whole-application benchmark. Its cold processing took hours. The current native runtime is configured for 262,144 tokens. Do not read that setting as a claim of instant full-window responses, universal workload fit or zero cold waits.

## Current limits

- Native EP installers provision the pinned release assets on Apple Silicon Macs and USB-connected ARM64 Android phones. OS approvals remain necessary; Windows/Linux installers and a general-purpose reference importer are not included.
- macOS ARM64 only in the tested environment. Portability work remains.
- The small model can choose an inappropriate tool, misunderstand a task or overstate a result. Receipts and checks reduce this; they do not eliminate it.
- Native canvas text controls now support selection, clipboard operations, caret movement and undo/redo. Broad font fallback, IME composition and accessibility semantics need further work. Stored Unicode can exceed what the installed font renders.
- Chat text is bounded to 6,000 UTF-8 bytes per submitted message. Larger material should use document tools; this input bound is separate from the model context capacity.
- Saved closed conversations are readable; the UI currently asks for a new conversation to continue, with relevant memory available through recall.
- PDF extraction is text-based; no general OCR pipeline or universal audio workflow is claimed.
- Offline Wikipedia/dictionary snapshots have dates. They are not fresh internet knowledge.
- Cached observations can become stale. Connected tools and explicit refresh may be needed.
- Application/browser tools depend on host permissions and supported interfaces. They do not provide universal desktop control.
- Local storage is private by location/permissions, not an application-level encrypted vault.

## Near-term priorities

1. Expand clean-install coverage across more Macs, Android manufacturers and network conditions.
2. Continue testing ordinary interaction paths, input methods and accessibility.
3. Improve recovery feedback and inspectable provenance in the UI.
4. Measure performance across representative native workloads and more machines.
5. Add optional adapters only with an actual execution path and validation.

### Source presentation

Consulted references appear in a collapsed Sources panel with readable titles, publisher labels, available snapshot dates, original-source links and Copy actions. New responses no longer receive a hardcoded URL appendix. Existing matching appendices are cleaned for display without rewriting saved history. The panel lists consulted references, not sentence-level support judgments. Original online pages may differ from the local snapshot used to answer. Focused validation passed 20 source-presentation/opening checks and 57 response-protocol checks.

### Automatic connectivity

Web and browser tools no longer require a mode switch. Existing offline settings are ignored, and the normal launcher no longer imposes an outbound-network sandbox on EchoLite or its child tools. This leaves macOS and browser permissions in place. Local inference, embeddings, memory, dictionary and installed Wikipedia remain independent of internet access.

Web requests reuse eligible cached content, attempt retrieval when needed, and fall back to saved pages on failure. Receipts expose the attempted request, failure, fallback and staleness without inventing a new validation time. Each later refresh may recover online automatically; an unreachable site does not establish that the whole machine is offline. Validation includes real HTTPS retrieval, test-process network denial with saved-page recovery, and successful retrieval after restoring access.

The automatic-connectivity change passed 95 focused checks. Two actual local-model conversations verified live web retrieval and saved-page fallback plus calculator use under test-process network denial. Managed Safari navigation/read/close also completed without a mode switch.

## Native installation and Android distribution

Two native EP setup apps now provision a Mac or a USB-connected Android phone. The APK has a mint adaptive icon, launcher entry and first-launch home-screen pin request. See [installer validation](INSTALLER-VALIDATION.md) for the actual checks and [setup](SETUP.md) for first-time OS approvals and disk requirements. Automatic USB/Wi-Fi sync and Android foreground-service restart checks are recorded in [background sync](android/BACKGROUND.txt).

## Prepared performance and storage update

The source now includes optional Android Adreno acceleration, persistent compiled GPU caching, queued sentence-safe neural speech, and protected cache maintenance. See [acceleration measurements](android/ACCELERATION.md), [storage retention](STORAGE.md) and [voice playback](VOICE.md). These are development-build results; the existing published release assets have not been replaced.

## Next update

EchoOffGrid integration is the next planned development phase, beginning with the ecosystem tab and source-linked library search, reading and opening. Broader workstation tools follow in stages. This release does not yet provide those integrations. See the [integration plan](../integration%20plan.txt).
