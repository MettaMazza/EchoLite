# Setup

EchoLite currently targets **macOS on Apple Silicon**. Development and local validation used an M1 Mac with 8 GB unified memory. Linux, Windows and Intel Mac support are not established.

This is a manual developer setup. The public repository deliberately excludes model weights, native runtime binaries, the reference library and personal state. A successful source build alone does not install these assets.

## 1. Install Ernos Plain

Use the installation/build instructions in [MettaMazza/Ernos-Programming-Language](https://github.com/MettaMazza/Ernos-Programming-Language). The local development compiler is installed at `~/.local/bin/ernos`, with its standard library at `~/.local/bin/stdlib`.

Install Apple's Command Line Tools if needed. In the EchoLite checkout, connect the compiler's standard library:

```sh
ln -s "$HOME/.local/bin/stdlib" stdlib
```

If your installation uses a different location, point the link there and set `ERNOS_COMPILER` to the compiler executable. The reference used during development was language repository revision `4b3ebd1359886999b4f043a4663b9470ee8d474c`; the language and application remain in development.

## 2. Provision native dependencies and model files

The layout below is what the current runtime code expects. See `config/native-*.json` for recorded versions, revisions, byte sizes and SHA-256 hashes. These manifests document the tested installation; editing them does not automatically change hard-coded runtime paths or flags.

```text
models/
  native/
    Qwen3.5-4B-Q4_K_M.gguf
    mmproj-Qwen3.5-4B-Q8_0.gguf
    all-minilm-l6-v2_f16.gguf
  runtime/
    llama-b12359/llama-server       (+ its release libraries)
    kiwix-tools_macos-arm64-3.8.2/kiwix-serve
    libraylib.dylib
    libgmp.10.dylib
```

| Dependency | Source / tested configuration |
| --- | --- |
| Generation and vision weights | [`ggml-org/Qwen3.5-4B-GGUF`](https://huggingface.co/ggml-org/Qwen3.5-4B-GGUF), revision pinned in `config/native-runtime.json` |
| Embedding weights | [`LLukas22/all-MiniLM-L6-v2-GGUF`](https://huggingface.co/LLukas22/all-MiniLM-L6-v2-GGUF), revision pinned in `config/native-embedding.json` |
| llama.cpp | [Release b12359](https://github.com/ggml-org/llama.cpp/releases/tag/b12359), macOS ARM64; preserve its dynamic libraries |
| Kiwix | [3.8.2 macOS ARM64 archive](https://download.kiwix.org/release/kiwix-tools/kiwix-tools_macos-arm64-3.8.2.tar.gz) |
| Raylib | [Upstream](https://github.com/raysan5/raylib), ARM64 dynamic library; the development library reports 6.2 |
| GMP | [6.3.0 source](https://ftp.gnu.org/gnu/gmp/gmp-6.3.0.tar.xz), ARM64 dynamic library; see `config/native-arithmetic.json` |

The UI also uses macOS system fonts and native frameworks. Model downloads are several gigabytes; allow additional space for persistent model-state caches. Dependency licenses apply independently of EchoLite's MIT license.

## 3. Provision local references

The current normal startup path expects a registered Wikipedia archive. The development installation uses `wikipedia_en_all_nopic_2026-06.zim` from [Kiwix's Wikipedia archive](https://download.kiwix.org/zim/wikipedia/), approximately **49 GiB** on disk, plus `library/wikipedia.json` with the identity/checksum metadata expected by `src/wikipedia.ep`.

The dictionary adapter expects `library/dictionary.sqlite3`, containing indexed English Wiktionary records and their compressed source payloads, with source, license and builder metadata. Source data came from [Kaikki/Wiktextract](https://kaikki.org/dictionary/rawdata.html). The expected database structure and readiness checks are visible in `src/dictionary.ep`.

**A general-purpose EP reference-library importer and automatic installer are not included in this source release.** Preparing these assets currently requires developer work; simply downloading a ZIM or an arbitrary dictionary database is not a complete setup. The app's reference-library bootstrap is an explicit next-development task, rather than an advertised one-command installation.

Optional local language documentation lives in `library/technical/ernos/`; it is read through file tools rather than loaded wholesale into model context. `ECHOLITE_LIBRARY` can select another prepared library directory.

## 4. Build and launch

```sh
~/.local/bin/ernos src/build.ep
./src/build
./echolite
```

The bootstrap installs the EP build entry point and compiles the native application binaries. Subsequent builds use `./build`. The compiler emits intermediate C; it is generated output, not the application's authored language.

`EchoLite.app` is another entry point after building. A fresh default state is created under `data/`; `ECHOLITE_HOME` selects another private state directory.

Local services bind to loopback: generation `8400`, embeddings `8401`, Wikipedia `8402`, and the optional managed Safari driver `8410`. The runtime verifies existing services before reusing them. It does not terminate arbitrary processes occupying these ports.

## Automatic connectivity

There is no manual mode switch. Web and browser tools are enabled at launch. Requests use eligible persistent cache entries or attempt online retrieval; failed requests fall back to retained snapshots when available, with stale dates and retrieval failures disclosed. With no cached page, the agent can use installed Wikipedia, dictionary, calculations and memory. Local model and embedding services remain on this machine.

Legacy `network-mode` files and `ECHOLITE_NETWORK_MODE` values no longer disable web access. The old CLI command only explains this migration. Restart an older running app once after upgrading to remove its inherited network restriction. Safari automation still requires Safari's remote-automation setting; connectivity recovery does not change OS permissions.

## Check the local state

```sh
./echolite init
./echolite doctor
./echolite chat demo "Use the calculator to calculate 47 * 19."
```

The calculator-backed demo should report **893** with a calculation basis. Keep `data/`, credentials, downloaded references and model-state caches private and out of Git.
