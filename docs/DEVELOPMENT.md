# Development

EchoLite's authored application and test code is Ernos Plain. Keep new runtime behavior, UI behavior and tests in `.ep`. Generated compiler output and separately licensed native dependencies are not application source.

## Build

Complete [setup](SETUP.md), then run `./build`. The bootstrap command is `~/.local/bin/ernos src/build.ep`, followed by `./src/build`. Never commit the generated binaries or `*_compiled.c` files.

## Focused tests

Tests are native EP executables. Compile a test and run it with the arguments stated in its source. Examples from the repository root:

```sh
~/.local/bin/ernos tests/memory_beliefs.ep
./tests/memory_beliefs

~/.local/bin/ernos tests/chat_editor.ep
./tests/chat_editor models/runtime/libraylib.dylib /tmp/echolite-editor.png

~/.local/bin/ernos tests/text_layout.ep
./tests/text_layout models/runtime/libraylib.dylib
```

The editor test briefly opens a native window, exercises the system text clipboard and restores its prior text. Do not use it while someone is actively copying sensitive content. Native font rendering requires a GUI session.

The deterministic offline regression entry point is:

```sh
~/.local/bin/ernos tests/acceptance.ep
./tests/acceptance /absolute/path/to/a-new-results-directory
```

Its output directory must not already exist. It uses isolated fixture states. Model, browser and UI journey tests have additional prerequisites; inspect their source before running them. Never point destructive/forgetting/recovery fixtures at a personal state directory. A full-context benchmark is not a routine regression test.

## Structure

```text
src/             Native EP application, tools and UI
src/chat_editor.ep   Composer and read-only text selection
src/echolite.ep      Conversation and persistence controller
src/native_recall.ep Embedding-backed retrieval
src/memory_beliefs.ep Belief assessments and applicability
config/          Recorded native dependency/model manifests
tests/           EP component and journey checks
docs/            Setup, architecture, status and operational guides
```

## Changes worth reviewing carefully

Memory migrations should preserve original IDs/text, provenance and forgetting semantics. Cache changes need an invalidation story. Side-effecting tools need explicit receipts and recovery behavior. UI work needs a visible interaction check, not just a running process or an off-screen render.

Keep performance claims scoped to measured builds and workloads. A configuration limit, a fixture pass and a real model journey are different kinds of evidence.
