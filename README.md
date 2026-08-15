# Local Model Research

Research on whether [Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) can run locally, starting with **KIS-laptop** and continuing on a desktop.

**Repo:** [cjessup/localModelResearch](https://github.com/cjessup/localModelResearch)
**Working directory name on both machines:** `localModel`

---

## Continue here (desktop session)

This work started on **KIS-laptop**. That machine is done. The next session is on a **desktop** that has not been inventoried yet.

If you are a new session in a folder named `localModel` (or this repo), do this:

1. **Do not re-analyze KIS-laptop.** That write-up is finished in [docs/KIS-laptop-and-model.md](docs/KIS-laptop-and-model.md).
2. **Inventory this desktop the same way:** CPU, RAM (size, speed, channels), every GPU (`nvidia-smi` if present, PCI devices, VRAM), disk free space, OS, page file, and any local-LLM tools already installed (Ollama, llama.cpp, LM Studio).
3. **Write the desktop report** as `docs/<desktop-name>-and-model.md`, using the computer’s hostname the same way KIS-laptop was used. Compare it to Qwen3.8-27B’s memory and speed needs (already documented; do not redo that research unless the model card has changed).
4. **Compare desktop vs KIS-laptop** for this model: can it load, what quant, what context, expected tok/s, thinking mode yes/no.
5. Update this README with a short desktop verdict and a link to the new doc.

This is research only. Do not build a website or an app unless asked.

### What is already known

| Item | Status |
|---|---|
| Model | Qwen3.8-27B — dense ~28B, vision, thinks by default (`xhigh`), 262k native context |
| Memory floor | 4-bit GGUF ~17–19 GB; 6-bit ~24 GB; 8-bit ~31 GB; BF16 ~56 GB. KV cache is extra and large |
| KIS-laptop | Dell Inspiron 16 7610, i7-11800H, **64 GB DDR4-3200**, **no discrete GPU**, Intel UHD only |
| KIS-laptop verdict | Can load Q4 from RAM at **~2–4 tok/s**. Not a good daily driver for this model’s thinking/agentic default |
| Used-hardware scan | [docs/craigslist-socal.md](docs/craigslist-socal.md) — OC / LA / SD Craigslist as of 2026-08-14. Sweet spot is **24 GB** VRAM; **32 GB** (5090) is the first comfortable thinking-mode card |

Prompt for the desktop session:

> Continue the localModelResearch work. KIS-laptop is already analyzed. Inventory this desktop and write `docs/<hostname>-and-model.md` comparing it to Qwen3.8-27B. Read README.md first.

---

## Documents

| Document | Contents |
|---|---|
| [docs/KIS-laptop-and-model.md](docs/KIS-laptop-and-model.md) | KIS-laptop inventory, model memory math, and a run/no-run verdict |
| [docs/craigslist-socal.md](docs/craigslist-socal.md) | Craigslist scan of OC, LA, and San Diego listings for a better box |

## KIS-laptop short answer

KIS-laptop **can load** a 4-bit Qwen3.8-27B GGUF from its 64 GB of system RAM. There is **no discrete GPU**, so generation will sit around **2–4 tokens/second**. That is usable for short, non-thinking chat and a poor match for this model’s default long thinking / agentic behavior.

A used **24 GB** card (RTX 3090 / 4090 / RX 7900 XTX) is the cost-effective upgrade. A **32 GB** RTX 5090 is the first card that makes thinking mode comfortable.
