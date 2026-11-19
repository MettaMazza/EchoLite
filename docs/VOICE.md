# Local neural voice playback

Each nonempty assistant reply has Copy and Read aloud controls below its content and sources. Copy places the complete response on the clipboard and briefly displays Copied. Read aloud uses Kokoro-82M v1.0 int8, voice `af_heart` (speaker 3), at normal speed. Stop reading cancels generation and playback. There is no system-TTS or cloud fallback.

Application code and native bindings are Ernos Plain. The downloaded neural inference dependencies are sherpa-onnx and ONNX Runtime. macOS plays generated WAV files through afplay; Android plays them through SDL3 audio. These players do not synthesize the voice.

## Assets and versions

- Model package: https://github.com/k2-fsa/sherpa-onnx/releases/download/tts-models/kokoro-int8-multi-lang-v1_0.tar.bz2
- Upstream model: https://huggingface.co/hexgrad/Kokoro-82M
- model.int8.onnx SHA-256: `75635ea3b85a32d00094dcdd61a258aba87c160f7c264c304a7b3ae9d734195e`
- Mac native runtime: sherpa-onnx v1.12.33, osx-arm64-shared package, ONNX Runtime 1.23.2.
- Android native runtime: sherpa-onnx v1.12.10 Android package, ONNX Runtime 1.17.1. The newer Android runtime tested on the REDMAGIC selected an unsupported SME instruction; the baseline runtime generated the same test successfully.

Extract the model package contents into `models/voice/kokoro/` in each installation. This includes the model, voices.bin, tokens, pronunciation lexicons, dict and espeak-ng-data. Mac dynamic libraries belong in `models/voice/runtime/`. Compile `src/speech_worker.ep` to `bin/echolite-speech` on Mac.

The Android build expects the baseline libraries under the build workspace's sibling `voice/android-baseline/jniLibs/arm64-v8a/` directory. It builds and packages the EP worker as libecholite-speech.so with the two speech runtime libraries. Model data remains in private app storage, outside the APK.

Downloaded assets retain their upstream licenses: Kokoro and sherpa-onnx are Apache-2.0; ONNX Runtime is MIT. The integrated eSpeak NG pronunciation dependency is GPL-3.0. These third-party components are separate from EchoLite's MIT application source; redistribution must retain and comply with their respective licenses. Model/runtime files are not committed to the source repository.

## Memory, cache and cancellation

A separate EP worker loads the voice model only for uncached generation and exits afterwards. Sentence-sized chunks keep individual waveforms bounded and allow playback before the complete reply is generated. Audio and request text are stored privately under ECHOLITE_HOME/speech-cache. Cache keys include exact text, voice, speed, chunking version, model and platform runtime. Completed WAV chunks are atomically renamed; unfinished chunks are regenerated. Cached replies play without loading the neural model. The cache persists across launches; no automatic disk quota is currently applied.

The initial 119-byte sample on the 8 GB M1 Mac generated in 8.7 seconds, using 270,123,008 bytes maximum resident memory and 331,090,304 bytes peak memory footprint. This is a measured sample, not a bound for all inputs. Mac regression checks exercise lossless Unicode chunking, real neural generation, playback completion, cached replay without a worker, and Stop. The same native worker generated two WAV chunks successfully on the REDMAGIC. Installed-app interaction checks are recorded separately when deployment completes.

## Installed-app check — 19 November 2026

The update was installed and reopened on both devices. Mac Copy produced a clipboard SHA-256 identical to the full response text; the installed app generated WAV audio and launched afplay. Stop reading returned to Read aloud and ended the playback process. Reply IDs now use string equality so action state survives database refreshes. On Android, tapping Read aloud generated two completed private cached WAV chunks and a completion marker. The user resumed active phone use before the final playback/Stop interaction check, so that particular check was not completed on-device.
