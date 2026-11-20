# Android acceleration and measured latency

The optional `libllama-opencl.so` runs the selected Qwen3.5-4B or Qwen3.5-9B profile through llama.cpp's Adreno OpenCL backend. The runtime manager chooses it when packaged and the vendor OpenCL driver is present. An accelerator startup failure falls back to the packaged CPU worker. Model choice stays 4B by default; context stays 262,144, KV types q4_0, with four recurrent checkpoints. The vision projector remains on CPU. Model workers run at nice level 5 to reduce competition with interactive work. Unload recognizes either backend using the app UID, executable path, model, credential path and port.

The tested device is REDMAGIC 11 Pro with Adreno 840 and approximately 24 GB physical RAM. Swap expansion is not counted as physical RAM. Other vendors and drivers need their own correctness and performance checks. A Vulkan candidate produced corrupt output and a separate float32 attempt lost its device; neither is packaged as a selectable backend.

## Evidence from 19 November 2026

| Check | Observed result |
| --- | --- |
| Persistent OpenCL compiled-program cache | Restart loaded 300 programs from disk: 300 hits, zero misses |
| Cache size | About 8 MiB; startup maintenance bounds it to 64 MiB / 4,096 generated programs |
| Real harness first greeting | 37.507 seconds; 1,827 prompt tokens, 212 generated tokens |
| Real harness follow-up | 22.909 seconds; 1,939 prompt tokens, 130 generated tokens |
| Follow-up KV reuse | Four saved checkpoints restored; only 407 of 1,939 prompt tokens evaluated (about 79% reused) |
| Prompt processing | 110.06 tokens/s first turn; 64.53 tokens/s for the follow-up suffix |
| Decoding in real harness | 10.70 and 8.82 tokens/s in those two runs |
| Unload/reload | Owned OpenCL process stopped; 4B, projector and embedding service restarted successfully |
| Explicit calculator round trip | `19*37` produced a completed calculator receipt of 703, followed by a calculation-based answer |
| System headroom during testing | 11,042,720 kB MemAvailable; thermal status 0 |

Earlier CPU runs of the same greeting sequence took 75.205 and 55.823 seconds. Generated text/token counts differed, so these are end-to-end observations, not a controlled universal speedup ratio. Actual reasoning traces were inspected in all runs. Private traces and databases are not repository artifacts.

Persistence does not guarantee every prefix is reusable. After process restart, a different arithmetic prompt restored its checkpoint file but evaluated all 1,948 prompt tokens because no retained recurrent checkpoint covered its reusable boundary. The subsequent warm follow-up tests demonstrate actual reuse, separately from file restoration. Four checkpoints cost about 201 MiB and bound this tradeoff.

A bare arithmetic request returned the correct number but selected experience without calling a calculator. The explicit calculator request exercised both tool dispatch and the subsequent answer successfully. This is a remaining answer-basis reliability issue; GPU acceleration does not resolve it. Keep it in the behavioral regression cases before ecosystem integration.

## Reproducible dependency build

Use the same pinned llama.cpp commit `b1b770d90d94cad9e9ea5b993bdbc383a522fb28`, build identity b12359, NDK r30, Android arm64 API 28, CMake 3.31.6 and Ninja. Configure a Release static build with:

- `GGML_OPENCL=ON`, `GGML_OPENCL_USE_ADRENO_KERNELS=ON`, `GGML_OPENCL_EMBED_KERNELS=ON`.
- `GGML_NATIVE=OFF`, `GGML_OPENMP=OFF`, `GGML_CPU_ARM_ARCH=armv8.7-a`.
- `LLAMA_BUILD_NUMBER=12359`, `LLAMA_BUILD_COMMIT=b1b770d9`, server enabled, examples/tests disabled.
- `OpenCL_INCLUDE_DIR` pointing to Khronos OpenCL-Headers revision `82f60316cc142e7b21dc1d8983b26de18858fbd9`.
- `OpenCL_LIBRARY` pointing to a locally obtained Android vendor library for linking. Do not package or redistribute that driver.

Place the resulting server in `ANDROID_WORK/build/llama-opencl/bin/llama-server`. The EP Android builder includes it if present, strips debug symbols and retains the separate CPU fallback. The app declares the optional native library and resolves the device's `/vendor/lib64` driver at worker startup. The upstream build uses its own kernel-generation scripts; EchoLite's authored application and lifecycle logic remain Ernos Plain.

## Optional 9B comparison

The optional 9B profile now uses the same OpenCL path. See [the matched phone comparison](MODEL_COMPARISON_2026-11-19.md) for timings, reasoning review, memory observations and cache evidence. 4B remains the default.
