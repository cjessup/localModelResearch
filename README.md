# Local Model Research

Research on whether [Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) can run on a current Dell Inspiron 16 7610, and what used hardware in Orange County / Los Angeles / San Diego is a reasonable next step.

**Date:** 2026-08-14

| Document | Contents |
|---|---|
| [docs/laptop-and-model.md](docs/laptop-and-model.md) | Laptop inventory, model memory math, and a run/no-run verdict |
| [docs/craigslist-socal.md](docs/craigslist-socal.md) | Craigslist scan of OC, LA, and San Diego listings for a better box |

## Short answer

The current laptop **can load** a 4-bit Qwen3.8-27B GGUF from its 64 GB of system RAM. There is **no discrete GPU**, so generation will sit around **2–4 tokens/second**. That is usable for short, non-thinking chat and a poor match for this model’s default long thinking / agentic behavior.

A used **24 GB** card (RTX 3090 / 4090 / RX 7900 XTX) is the cost-effective upgrade. A **32 GB** RTX 5090 is the first card that makes thinking mode comfortable.
