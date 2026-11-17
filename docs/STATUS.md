# Development status and limits

**Active development / experimental source release.** The application runs on its provisioned development Mac. Tests and demo journeys are useful evidence, not a promise that every interaction on every machine is finished or reliable.

## What has been exercised

- Local model/tool conversations, calculator answers and local reference use.
- Embedding recall, prior-session continuity, corrections and explicit forgetting.
- Versioned Me edits, actual tokenizer limits and next-turn activation.
- Task state, schedules, interruption, process output recovery and deliberate retries.
- Native chat streaming, a thinking dropdown, image input and document adapters.
- Browser create/navigate/read/close and launching a local application.
- Backup/restore recovery and failure boundaries.
- Belief assessments and time-based invalidation of cached recall.

The last full deterministic offline regression before the latest editor work passed **35 suites**. Focused editor work added **20 native editor checks** and **27 font-layout checks**, including real clipboard round trips, Unicode byte boundaries, insertion/selection replacement, bounded undo/redo and visible selection rendering. These tests do not certify complete macOS accessibility or every physical input sequence. The interface had a hidden-window launch bug and lacked normal editing controls; those reports prompted fixes. UI polish remains ongoing.

The public screenshot shows an actual local Qwen greeting, with the visible reply exactly matching the model output. Its preview is limited to that verified turn. Earlier calculator journeys returned `47 × 19 = 893` using tool receipts; those are separate checks, not claims about the greeting screenshot.

## Performance evidence has a scope

Development used an M1 Mac with **8 GB unified memory**. One recorded ordinary offline UI journey sampled peak simultaneous process RSS at approximately **3.019 GiB**. RSS is not a complete measure of GPU allocation, physical footprint or unrelated applications.

A retained full-context experiment processed **262,112 prompt tokens plus a 32-token output reserve**, including warm and disk-restart reuse. That older experiment used the earlier MLX harness; it is **not** a fresh native llama.cpp whole-application benchmark. Its cold processing took hours. The current native runtime is configured for 262,144 tokens. Do not read that setting as a claim of instant full-window responses, universal workload fit or zero cold waits.

## Current limits

- Manual dependency/library provisioning; no finished installer or general-purpose reference importer in this release.
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

1. Make first-time provisioning reproducible and easier.
2. Continue testing ordinary interaction paths, input methods and accessibility.
3. Improve recovery feedback and inspectable provenance in the UI.
4. Measure performance across representative native workloads and more machines.
5. Add optional adapters only with an actual execution path and validation.
