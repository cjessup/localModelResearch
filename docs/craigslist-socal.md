# Craigslist scan: OC / LA / San Diego

Research date: 2026-08-14

Goal: find a **good cost/performance** used computer for [Qwen3.8-27B](https://huggingface.co/Qwen/Qwen3.8-27B) on Craigslist in Orange County, Los Angeles, and San Diego.

The current laptop cannot take a desktop GPU. Anything purchased has to be a **whole PC**, or a GPU **plus** a desktop.

## What to buy for this model

| Goal | VRAM | Expected Qwen3.8-27B result |
|---|---|---|
| Barely works | 16 GB (4080 / 5070 Ti / 5080 / laptop 4090) | Q4, short context, thinking off or `low` |
| The sweet spot | **24 GB** (3090 / 4090 / 7900 XTX) | Q4–Q5, 16–32k context, ~30–60 tok/s |
| Comfortable | **32 GB** (5090) | Q5–Q6, longer context, thinking usable |
| Skip | 8–12 GB (3050 / 3060 / 3080 10 GB / 4060) | Worse than the current 64 GB laptop in some ways |

Craigslist in these three counties was **thin on true 24 GB boxes** on this date. A lot of “gaming PC” ads are 8–16 GB cards that will load the model and then choke on context and thinking.

Approximate used-market context (not Craigslist-specific): 3090s around $900–$1,300; 4090s still well above $1,000 used; 5090 systems mostly $5,500+.

## Best matches found

### 1. Best complete “good enough” system — Los Alamitos, $3,000

[Ryzen 9 7950X3D + RX 7900 XTX 24GB + 64GB DDR5 + 6TB + two monitors](https://www.craigslist.org/view/d/los-alamitos-ultimate-high-end-gaming/6jQq48a4M5WWAUouL9rJXx)

- 24 GB VRAM is the number that matters. This was the only complete 24 GB machine found in the three counties at a sane price.
- 64 GB RAM matches the current laptop, so offload is easy if context grows.
- AMD, so run it in **llama.cpp / LM Studio (Vulkan)**, not vLLM/CUDA. For local chat that is fine.
- Two monitors are bundled. If they are not needed, a reasonable offer is **$2,400–$2,600** for the tower only.

This is the listing to drive to first if the budget is about $3k.

### 2. Best performance if spending more — Central LA, $5,500

[Alienware Area-51, Ryzen 9 9950X3D, RTX 5090 32GB, 32GB RAM, 2TB](https://www.craigslist.org/view/d/los-angeles-dell-alienware-area-51/qJc8HamtH7J9bTTVLimPS5)

- **32 GB VRAM** is the first card on this list that makes Qwen3.8-27B comfortable: higher quant, more context, thinking mode without immediately running out of memory.
- 9950X3D is excellent. 1500 W PSU is appropriate for a 5090.
- Only 32 GB system RAM. That is okay because the model lives on the GPU, not in system RAM.
- Seller said they paid about $7k for a flight-sim box. $5,500 is not a steal, but it is the best *performance* deal that was listed.

San Diego has two similar 5090 towers at [$6,480](https://www.craigslist.org/view/d/san-diego-new-amd-ryzen-x3d-16-core/1a8ALat7T9m6BrB4qdiSqE) and [$6,600](https://www.craigslist.org/view/d/san-diego-high-end-workstation-gaming-pc/rLqVvMfS3bRnT3LDHkHdvY). Same idea, more money. The Laguna Hills 5090 at [$8,000](https://www.craigslist.org/view/d/mission-viejo-high-end-rtx-5090-pc-8tb/vqPkeZXuZ4ttYrBpRPGsCb) is not a deal.

### 3. Best cheap path if assembling — Tarzana $1,000 + a cheap used desktop

[RTX 3090 24GB, $1,000, Tarzana](https://www.craigslist.org/view/d/tarzana-nvidia-geforce-rtx-3090/bATKWWASNHFpZVKURNm9gQ)

Used 3090s are still the local-LLM value card: 24 GB, CUDA, and this one is priced around market, not “too good to be true.”

A tower is still required, with:

- 750–850 W+ PSU (preferably 850–1000 W gold)
- a free PCIe x16 slot
- ideally 32–64 GB RAM

A plain used AM4/AM5 desktop on Craigslist is often $300–$700. All-in that is around **$1,400–$1,800** for a real 24 GB Qwen box. That is the best dollars-per-token on this scan.

Nearby GPU-only alternatives:

- [EVGA 3090 Ti 24GB, Irvine, $1,500](https://www.craigslist.org/view/d/tustin-evga-geforce-rtx-3090-ti-ftw3/r5tWzKqzeMwsQRX7i6mr3M) — same 24 GB, sitting since June, overpriced vs the $1,000 3090.
- [3090 FE, Santa Clarita, $1,600](https://www.craigslist.org/view/d/santa-clarita-nvidia-rtx-3090-fe/sUUqs3bPk9bzTUqo2u1Tjp) — skip.

### 4. Quiet Apple option — Hollywood Hills, $2,050

[Mac Studio M1 Max, 64GB, 2TB, AppleCare+ remaining](https://www.craigslist.org/view/d/los-angeles-m1-max-mac-studio-loaded/7WL5JmfRYjhWcVEfiT49DK)

- Unified 64 GB will run a 4-bit 27B much faster than the DDR4 laptop (ballpark **15–25 tok/s**, not 2–4).
- Context still limited. Thinking-heavy runs will still eat RAM.
- Quiet, simple, no PSU/GPU lottery.
- The East Irvine [M1 Max 32GB at $1,300](https://www.craigslist.org/view/d/east-irvine-apple-mac-studio-m1max/6gnGpVR7JUaohmVaxRwNis) is **too small** for this model.

There is also an [M3 Ultra 96GB / 16TB at $4,199](https://www.craigslist.org/view/d/los-angeles-apple-mac-studio-m3-ultra/6T9eirbcsY1SMiAw1AznfH) in Hollywood Hills. If genuine, that would run this model very well. The ad reads like generated IT-surplus copy and offers delivery. Treat it as **verify in person or walk away**.

## Reasonable but tight (16 GB)

These will run Q4 with short context. They will not feel “good” once the model starts thinking.

| Listing | Price | Why it is only okay |
|---|---|---|
| [MSI Aegis, 9900X + RTX 5080 16GB, San Marcos](https://www.craigslist.org/view/d/san-marcos-new-msi-aegis-ryzen-9900x/o6iRHgnqT8NsehfcRSWLk2) | $2,700 | Fast 16 GB. Need to add RAM (max 96 GB). |
| [Ryzen 7 7800X3D + 5070 Ti, Costa Mesa](https://www.craigslist.org/view/d/costa-mesa-gaming-pc-for-sale-geforce/eMvZkSj7GWw8KRnZ4QNdbT) | $2,100 | Same 16 GB limit, fair-ish used price. |
| [4080 Suprim, Los Alamitos](https://www.craigslist.org/view/d/los-alamitos-geforce-rtx-4080-suprim/5ir1Es4Hu1YRaMHDEacCvi) | $800 | GPU only, 16 GB. Cheap if a desktop already exists. |
| [ROG Strix 4080, Newport](https://www.craigslist.org/view/d/newport-beach-rtx-4080-oc/n1NJw3X3QE5i5QXBKb2cWd) | $1,000 | Same story. |

Do **not** buy the Anaheim [5070 Ti / 16 GB RAM / 1 TB](https://www.craigslist.org/view/d/anaheim-brand-new-high-end-4k-gaming-pc/3uFcvN9BaCFjsyHDbYYQ6F) at $2,000. 16 GB system RAM plus 16 GB VRAM is a bad combo for this model.

## Treat as scams or bad value

| Listing | Why |
|---|---|
| [4090, Anaheim, $400](https://www.craigslist.org/view/d/anaheim-nvidia-geforcertx4090/kKcE2XEPd8doc54AQjAsff) | New 4090 at $400, crypto + delivery. Classic scam. |
| [4090, Downey, $700](https://www.craigslist.org/view/d/downey-gigabyte-geforce-rtx-gb-gddr6x/b24bJRtoc5oysh6oNpPnUP) | Used 4090s are still ~$1,100–$2,500. Crypto + delivery. |
| [Zephyrus Duo 16 “4090” 64GB 4TB, $1,150](https://www.craigslist.org/view/d/los-angeles-asus-rog-zephyrus-duo-16/8vxtrorP1B9U1UGmZYws7L) | That laptop is a $3k–$4k machine. Also laptop 4090 is **16 GB**, not 24. Only consider cash, in person, powered on. |
| [Custom PC, Fountain Valley, $3,000](https://www.craigslist.org/view/d/fountain-valley-custom-built-pc/b5GHBgeNqcaoUb2vevRwUL) | Ryzen 5 + **RTX 3050 8GB**. Worse than the current laptop. |
| [Lake Forest “PC with RTX 3090”, $3,000](https://www.craigslist.org/view/d/mission-viejo-pc-with-rtx-tb-ssd-64gb/sXRrDp9Lyc2bm9wDydjDa3) | No photos, no CPU listed. $3k for an unknown 3090 box is not a deal. |
| Most 5060 / 4060 / 3060 gaming PCs | 8–12 GB. Wrong class for a 27B dense model. |

**San Diego note:** [BIZON Threadripper 3960X, 192 GB RAM, RTX 3080 10GB, $1,400](https://www.craigslist.org/view/d/san-diego-bizon-x5000-g2-workstation-pc/mL8QH8SbQ6srtEe743DpYp) is a lot of CPU/RAM for the money, but the **10 GB GPU cannot hold this model**. Only interesting if a 3090 will be dropped into it (confirm the PSU first).

## Recommended next step by budget

1. **About $1,500–$2,000:** Message the [Tarzana 3090](https://www.craigslist.org/view/d/tarzana-nvidia-geforce-rtx-3090/bATKWWASNHFpZVKURNm9gQ). Pair it with any cheap used desktop that has an 850 W PSU. That is the cheapest way to go from “2 tok/s on the laptop” to a real local 27B.
2. **About $3,000, want it to just work:** [Los Alamitos 7900 XTX tower](https://www.craigslist.org/view/d/los-alamitos-ultimate-high-end-gaming/6jQq48a4M5WWAUouL9rJXx). Negotiate the monitors off.
3. **About $5,500 and thinking mode should be usable:** [Alienware 5090](https://www.craigslist.org/view/d/los-angeles-dell-alienware-area-51/qJc8HamtH7J9bTTVLimPS5).
4. **Do not** buy another 16 GB gaming laptop or a 3050/4060 desktop. That spends money and stays in the same “it loads, it crawls” situation.

Craigslist SoCal was a weak market for this on 2026-08-14. If none of the 24 GB options are still up, Facebook Marketplace usually has more used 3090 towers in OC/LA than Craigslist does.

Listings move quickly. Confirm the ad is still live and test the machine in person (cash, powered on, `nvidia-smi` or GPU-Z showing the claimed VRAM) before paying.
