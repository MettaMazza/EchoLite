# Android 4B and 9B comparison — 19 November 2026

Method for the 2026-11-19 phone comparison

Device: REDMAGIC 11 Pro, Adreno 840, approximately 24 GB physical RAM. Swap expansion is not counted as RAM. Tests ran sequentially on the same connected, awake phone; unrelated apps were left running. Android reported thermal status 0, but frequency/throttling was not independently controlled. These are observed application-device results, not laboratory model-quality rankings.

Models: installed Qwen3.5-4B-Q4_K_M and Qwen3.5-9B-Q6_K. This compares the user's actual options, not equal-bit quantizations. Both used llama.cpp b12359 OpenCL, all requested layers on GPUOpenCL, CPU vision projector, full configured context 262144, q4_0 KV, flash attention, batch 256 / microbatch 64, four recurrent checkpoints, four threads, nice 5, one slot.

Direct API cases used the same system/user messages, seed 42, temperature 1, top_p .95, top_k 20, min_p 0, presence_penalty 1.5, repeat_penalty 1, streaming, cache_prompt true and max_tokens 1536. TTFT means first generated reasoning/content token, not the first visible final-answer token. The shared system message was: You are EchoLite, a local assistant. Answer concisely. Use provided tool results as evidence. If tools are needed, request them rather than inventing results.

Prompts:
- Hello!
- The calculator tool returned {"expression":"19*37","result":703}. Tell me the answer in one sentence and say how it was obtained.
- Three labelled boxes contain only apples, only oranges, and a mix. Every label is wrong. You may draw one fruit from one box. Which labelled box should you choose, and how can you identify all three? Explain briefly.

This short common system message isolates model behavior; it is not EchoLite's full identity, recall or action schema. The calculator-result case supplies evidence and does not demonstrate a real tool dispatch. A separate fresh-state EchoLite-core calculator round trip checks actual dispatch, persisted receipts, structured answers and reasoning.

The 4B greeting had a cached prefix after a diagnostic script compatibility failure; its first retained timing is not a cold-cache result. The warmed greeting was repeated for both models. Extra repeated puzzle (4B) and calculator-result (9B) requests were cancelled to avoid redundant long runs and excluded from completed-case statistics. Completed or token-capped cases are reported individually. A cap is not a successful answer. No general quality claim follows from this small prompt set.

Private raw traces and diagnostic databases are retained outside the repository. Cache hits are reported from runtime cache_n/prompt_n rather than snapshot existence.

## Direct API results

| Model | Case | Run | Total seconds | First token seconds | Generated tokens | Decode tokens/s | Cached prompt tokens | Finish |
| --- | --- | --- | ---: | ---: | ---: | ---: | ---: | --- |
| 4b | greeting | 1 | 68.82 | 0.22 | 738 | 10.74 | 47/51 | stop |
| 4b | greeting | 2 | 77.38 | 0.26 | 738 | 9.56 | 47/51 | stop |
| 4b | reasoning | 1 | 271.49 | 1.44 | 1536 | 5.68 | 39/95 | length |
| 4b | tool_result | 1 | 85.28 | 0.87 | 826 | 9.77 | 39/82 | stop |
| 4b | tool_result | 2 | 88.74 | 0.31 | 826 | 9.33 | 78/82 | stop |
| 9b | greeting | 1 | 48.63 | 2.05 | 250 | 5.35 | 0/51 | stop |
| 9b | greeting | 2 | 84.47 | 0.4 | 250 | 2.96 | 47/51 | stop |
| 9b | reasoning | 1 | 542.9 | 1.98 | 1536 | 2.84 | 39/95 | length |
| 9b | tool_result | 1 | 245.0 | 1.88 | 729 | 2.99 | 39/82 | stop |

Both calculator-result answers were correct. Both puzzle runs reached the 1536-token test cap with no final answer. Their inspected traces correctly chose the Mix-labelled box initially, then repeatedly confused the remaining label assignments. Neither capped result counts as a completed solution. The greeting and calculator traces repeatedly polished simple wording; caching cannot eliminate the cost of newly generated reasoning.

## Runtime and memory

9B initialized at context 262144 with its F16 vision projector, the GPU worker and 300 persistent OpenCL program-cache hits, zero misses. Startup to listening took 60.2 seconds. This is startup evidence, not a repeated full-window prompt-capacity sweep. Available memory was about 10.4 GiB during the 4B measurements and 5.9–6.1 GiB with 9B. Neither sampled generation process had swapped pages. CPU RSS excludes important GPU allocations and is not presented as total model memory.

## Actual EchoLite calculator pipeline

Fresh isolated local test homes used the same request: “Use the calculator tool to multiply 19 by 37 and report its result.” These runs used the real EP core, identity/context assembly, action schema, embeddings, tool dispatch, persistent result and slot caches, and normal app generation settings (8192 completion ceiling). They were not the short synthetic system-prompt tests above. Sampling was the ordinary app configuration rather than a fixed seed, so generated token counts differ.

| Measurement | 4B Q4_K_M | 9B Q6_K |
| --- | ---: | ---: |
| End-to-end seconds | 65.598 | 89.709 |
| Tool-request prompt / output tokens | 1830 / 172 | 1833 / 77 |
| Tool-request prefill tokens/s | 70.62 | 53.38 |
| Tool-request decode tokens/s | 8.75 | 4.92 |
| Answer prompt / output tokens | 1913 / 106 | 1914 / 112 |
| Answer decode tokens/s | 8.90 | 3.67 |
| Follow-up prompt tokens evaluated | 306 | 304 |
| Follow-up prompt reuse | 84.0% | 84.1% |
| Persisted calculator receipt | completed, 703 | completed, 703 |
| Final structured basis | calculation | calculation |

Both saved reasoning traces correctly requested the calculator, then used its receipt. 9B used fewer tokens to request the tool, but remained about 37% slower end to end in this run. Both restored four persisted recurrent checkpoints between rounds; the runtime timings demonstrate actual prefix reuse separately from restoration. Checkpoints occupied about 201 MiB.

## Decision and installed state

Keep 4B as the default. This small comparison does not demonstrate a quality improvement that offsets 9B's latency and memory cost. Both profiles now use the existing optional OpenCL path when selected, with the existing CPU startup fallback and available-memory admission checks. The change does not alter profile preference or context size.

The updated APK was signature-verified and installed in place. The app was reopened; authenticated health and props confirmed the resident default is Qwen3.5-4B-Q4_K_M at context 262144. Generation, embedding and Wikipedia services were ready. All 17 existing model-profile checks passed, including default selection and memory-pressure fallback. Personal conversations and EchoOffGrid were not modified.
