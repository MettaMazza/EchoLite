# Third-party components

EchoLite's MIT license covers its authored code and original documentation/graphics. It does not relicense third-party software, model weights or reference content. Model weights and reference archives are excluded from the source repository. The prepared installers bundle selected native dependencies and retrieve the remaining assets from pinned upstream URLs. Their separate notices are retained in android/assets/third-party and in the release payload.

| Component | Upstream |
| --- | --- |
| Ernos Plain compiler and standard library | https://github.com/MettaMazza/Ernos-Programming-Language |
| llama.cpp | https://github.com/ggml-org/llama.cpp |
| Qwen3.5 model family / downloaded GGUF conversion | https://huggingface.co/ggml-org/Qwen3.5-4B-GGUF |
| all-MiniLM-L6-v2 | https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2 |
| MiniLM GGUF conversion | https://huggingface.co/LLukas22/all-MiniLM-L6-v2-GGUF |
| Raylib | https://github.com/raysan5/raylib |
| GMP | https://gmplib.org/ — the recorded build uses LGPL-3.0-or-later; see its distribution terms |
| Kiwix / libzim | https://github.com/kiwix/kiwix-tools and https://github.com/openzim/libzim |
| Wikipedia content | https://www.wikipedia.org/ — retain article/contributor attribution and applicable content terms |
| Wiktionary / Wiktextract source data | https://kaikki.org/ and https://www.wiktionary.org/ |

Consult the exact upstream revision or distribution for its license and notices before redistributing dependencies. The application also uses system-provided libraries and frameworks, including SQLite, libffi, libcurl, libxml, libarchive and macOS graphics/document frameworks.

## Packaged Android and speech dependencies

| Component | Recorded build / upstream |
| --- | --- |
| SDL | 3.4.20 — https://github.com/libsdl-org/SDL |
| Android raylib | 6.2 — https://github.com/raysan5/raylib |
| libffi | 3.8.1 — https://github.com/libffi/libffi |
| OpenSSL | 3.5.9 — https://github.com/openssl/openssl |
| curl | 8.23.0 — https://curl.se/ |
| libxml2 | 2.15.4 — https://gitlab.gnome.org/GNOME/libxml2 |
| SQLite | 3.54.1 — https://sqlite.org/ |
| libzim / libkiwix | Android 9.8.2 / 14.2.1 — https://github.com/openzim/libzim and https://github.com/kiwix/libkiwix |
| ICU data | 73.2 — https://github.com/unicode-org/icu |
| Android platform tools | 37.0.1 — upstream NOTICE included with adb |
| Kokoro voice | Kokoro-82M v1.0 int8 — https://huggingface.co/hexgrad/Kokoro-82M |
| sherpa-onnx | Mac 1.12.33 / Android 1.12.10 — https://github.com/k2-fsa/sherpa-onnx |
| ONNX Runtime | Mac 1.23.2 / Android 1.17.1 — https://github.com/microsoft/onnxruntime |
| eSpeak NG | Pronunciation engine/data supplied with the speech distribution — https://github.com/espeak-ng/espeak-ng |

The speech distribution includes GPL-3.0 eSpeak NG components; Kiwix/libzim and GMP also have copyleft terms. EchoLite's MIT license does not replace these terms. Keep the component notices and upstream source references with binary distributions. The dictionary retains Wiktionary contributor attribution and CC BY-SA source metadata in its database; Wikipedia results retain article URLs and the snapshot date.
