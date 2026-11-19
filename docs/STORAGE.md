# Local storage and retention

Memory → Storage reports generated speech usage and offers Trim cache. The background sync worker also runs maintenance at startup and hourly, so cleanup does not depend on keeping the chat screen open.

| Data | Retention |
| --- | --- |
| Generated Kokoro audio and its temporary files | 512 MiB or 256 reply groups; oldest unused groups are removed first |
| Cached inference results | Existing 64 MiB / 1,024-entry budget |
| Compiled OpenCL GPU programs | 64 MiB / 4,096 programs; trimmed before accelerated-worker startup |
| Native KV checkpoints | One current persisted slot snapshot; replaced as conversation state advances |
| Runtime and sync diagnostic logs | When a log exceeds 8 MiB, retain its recent approximately 4 MiB tail |
| Conversations, saved reasoning, tool receipts, memories, Me history, attachments, downloaded models, reference library and backups | Preserved by automatic cache maintenance |

Generated audio is reproducible from the saved reply. Its exact text, model, voice, runtime and sentence-chunking version determine its cache identity. Entire reply groups are evicted together. Playback and synthesis hold a shared lease; cleanup needs an exclusive lease and retries later if audio is active. Unknown files, symlinks and multiply linked files are retained. No content keywords are used to decide what to retain.

Storage cleanup and semantic memory consolidation are separate. Existing consolidation previews, source lineage, protection and archival controls remain responsible for memory changes. Automatic cache cleanup does not silently summarize or erase original conversations. Protected user data can consequently grow over time; the cache quota is not a quota on a person's history.

The EP command-line entrypoints are `storage-report` and `storage-clean`, using the existing private `ECHOLITE_HOME`. A busy audio cache is reported explicitly. The Android installed-app check reported six cached replies using 19,040,825 bytes; trimming correctly removed nothing while below budget. Isolated quota-exhaustion tests verified eviction while preserving identity, unknown files and symlink targets; the active-playback lease prevented deletion.
