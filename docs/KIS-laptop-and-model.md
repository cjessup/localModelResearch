# Qwen3.8-27B on KIS-laptop

Research date: 2026-08-14

This session is hardware research, not an app build. The question is whether [Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) can run locally on the machine in front of us, and what that would actually feel like.

## Laptop inventory

Measured on the machine (`KIS-LAPTOP`), not taken from a spec sheet.

| Resource | This laptop | Why it matters |
|---|---|---|
| Machine | Dell Inspiron 16 7610 | 11th-gen H-series laptop |
| CPU | Intel Core i7-11800H, 8 cores / 16 threads | Fine for `llama.cpp` CPU inference |
| RAM | **64 GB DDR4-3200** (2×32 GB SODIMMs) | This is what makes 27B possible |
| Free RAM at inspect time | ~42 GB of 63.7 GB | Windows was already using ~22 GB |
| GPU | Intel UHD Graphics only | No CUDA, no useful dedicated VRAM |
| Discrete GPU | **None present** | No NVIDIA/AMD device in the PCI tree, including hidden devices. `nvidia-smi` is not installed |
| Storage | ~1.9 TB NVMe, **~1.65 TB free** | Plenty of room for 18–35 GB GGUF files |
| OS | Windows 11 Pro | Ready for LM Studio / Ollama / llama.cpp |
| Existing local-LLM tools | None installed | Clean slate |
| Page file | ~4 GB | Too small if a large context spike hits |

The Inspiron 16 7610 *can* ship with a GTX 1650 or an RTX 3050/3060. This unit does not have one attached. The iGPU is Tiger Lake UHD (shared system RAM), so it does not give a separate 8–16 GB VRAM pool.

Theoretical memory bandwidth is about **51 GB/s** (DDR4-3200, dual channel). That number, not core count, sets generation speed for a dense 27B.

## What Qwen3.8-27B actually needs

Qwen3.8-27B is a **dense ~28B** model (not MoE). The whole weight file has to sit in RAM or VRAM. It is also multimodal, thinks by default (`reasoning_effort=xhigh`), and has a native **262k** context.

Official card: [Qwen/Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) (Apache 2.0). Community GGUFs: [unsloth/Qwen3.8-27B-GGUF](https://huggingface.co/unsloth/Qwen3.8-27B-GGUF). Unsloth run notes: [How to run Qwen3.8](https://unsloth.ai/docs/models/qwen3.8).

Unsloth’s published memory floor (weights + a little headroom, **before** a large KV cache):

| Quant | Memory needed | Fits in 64 GB? |
|---|---|---|
| 2-bit | 11–13 GB | Yes, quality drop |
| 3-bit | 13–16 GB | Yes |
| **4-bit (recommended)** | **17–19 GB** | **Yes, comfortably** |
| 6-bit | ~24 GB | Yes, if other apps are closed |
| 8-bit | ~31 GB | Tight with Windows already using 22 GB |
| BF16 (full) | ~56 GB | Loads barely; almost no room for OS + KV cache. Skip it |

Typical 4-bit GGUF files are **~17–18 GB** (`UD-Q4_K_XL`). Official BF16 weights are ~54–56 GB.

KV cache is the other budget. Community reports say a **16k context on this 27B can take well over 20 GB by itself** at higher cache precision. The 262k native window is not something this laptop can use.

## Speed math

Token generation on CPU is memory-bandwidth bound:

```text
tokens/s ≈ memory bandwidth / model size
```

For a ~18 GB 4-bit file on 51 GB/s DDR4:

- Theoretical ceiling: **~2.8 tok/s**
- Realistic `llama.cpp` CPU: **about 2–4 tok/s**
- Multi-token prediction (Qwen3.8 ships with MTP) might push that toward the high end if acceptance is good
- Prompt processing will also be sluggish versus a real GPU

That is usable for short answers. It is a problem for this model’s default behavior. Qwen3.8-27B **thinks first**, and at `xhigh` people have seen **14k thinking tokens + 7k output**. At 2 tok/s that is on the order of **hours for one hard prompt**.

Compare with GPU boxes people are actually using for this model:

- RTX 2080 Ti 22 GB: ~35 tok/s (Q4)
- RTX 3090 24 GB: ~50 tok/s with MTP
- 12 GB laptop 5070 Ti: ~4.5 tok/s (already GPU-bound, but still faster than this CPU path)

This laptop is on the CPU side of that chart.

## What is realistic on this machine

**Will load and answer**

- 4-bit GGUF (`UD-Q4_K_XL` / `Q4_K_M`), 8k–16k context
- Thinking **off**, or `reasoning_effort=low`
- Chat, short coding questions, summarization

**Will load, but get painful**

- 6-bit if browsers and other apps are closed
- Medium/high thinking
- 32k context

**Do not bother**

- Full BF16 / Hugging Face Transformers / vLLM / SGLang (those want NVIDIA)
- Native 262k context
- Default `xhigh` thinking for everyday use
- Video understanding (the model can do it; this CPU will crawl)

**Sweet spot if trying it on this laptop**

- App: LM Studio, Ollama, or llama.cpp (Vulkan on Intel UHD will not help much; same RAM bandwidth)
- Quant: Unsloth `Qwen3.8-27B` `UD-Q4_K_XL` (~18 GB)
- Context: **8k**, maybe 16k
- Thinking: off for chat; `low` only when needed
- Plug in, high-performance power plan, close heavy apps first
- Bump the page file from ~4 GB to **16–32 GB** so a spike does not crash the process

## Verdict

| Question | Answer |
|---|---|
| Can it run Qwen3.8-27B? | **Yes**, quantized, from system RAM |
| Is 64 GB enough? | **Yes** for 4-bit, maybe 6-bit |
| Is the GPU enough? | **No discrete GPU.** Intel UHD does not change the picture |
| Daily driver for this model? | **Only for short, non-thinking use.** The model is built for long agentic/thinking runs, and this CPU cannot feed that |
| Bottleneck | DDR4 bandwidth + no CUDA GPU, not disk and not raw RAM capacity |

The 64 GB upgrade is what makes this even on the table. A typical 16 GB Inspiron 7610 could not do this. What is still missing is a 16–24 GB NVIDIA card, which is what turns “it loads” into “it is pleasant.”

See [craigslist-socal.md](craigslist-socal.md) for used machines in Orange County, Los Angeles, and San Diego that would actually run this model well.
