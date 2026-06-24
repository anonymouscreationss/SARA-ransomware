<div align="center">

```
███╗   ███╗███████╗███████╗██████╗ ██╗   ██╗
████╗ ████║██╔════╝██╔════╝██╔══██╗██║   ██║
██╔████╔██║█████╗  █████╗  ██████╔╝██║   ██║
██║╚██╔╝██║██╔══╝  ██╔══╝  ██╔══██╗██║   ██║
██║ ╚═╝ ██║███████╗███████╗██║  ██║╚██████╔╝
╚═╝     ╚═╝╚══════╝╚══════╝╚═╝  ╚═╝ ╚═════╝
```

**by Skynetfc**

*An independent AI system built entirely from scratch.*
*No borrowed weights. No external AI APIs. Every line of intelligence was earned.*

---

[![Python](https://img.shields.io/badge/Python-3.11%2B-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://python.org)
[![PyTorch](https://img.shields.io/badge/PyTorch-2.2%2B-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white)](https://pytorch.org)
[![License](https://img.shields.io/badge/License-MIT-22C55E?style=for-the-badge)](LICENSE)
[![Made by Skynetfc](https://img.shields.io/badge/Made%20by-Skynetfc-6366F1?style=for-the-badge)](https://github.com/skynetfc)
[![Parameters](https://img.shields.io/badge/Parameters-85M-F59E0B?style=for-the-badge&logo=openai&logoColor=white)](#architecture)
[![Context](https://img.shields.io/badge/Context-1024%20tokens-0EA5E9?style=for-the-badge)](#architecture)
[![No APIs](https://img.shields.io/badge/External%20AI%20APIs-None-DC2626?style=for-the-badge)](#)

---

</div>

## What is Meeru?

Meeru is a decoder-only transformer language model built **completely independently from scratch** by Skynetfc. It exists nowhere else in the world. Every weight was earned through training — not downloaded, not fine-tuned from another model, not wrapped around any existing AI system.

- ❌ No OpenAI
- ❌ No Anthropic / Claude
- ❌ No Hugging Face pretrained weights
- ❌ No Llama, Mistral, or any other base model
- ✅ Pure PyTorch, written line by line
- ✅ Own BPE tokenizer, trained on its own corpus
- ✅ Own training loop, optimizer, and data pipeline

---

## Specifications

<div align="center">

| Specification | Value |
|:---|:---|
| 🧠 Parameters | ~85 million |
| 📏 Context Length | 1,024 tokens |
| 📚 Vocabulary | 16,000 BPE tokens |
| 🏗️ Transformer Layers | 12 |
| 👁️ Attention Heads | 16 (GQA: 8 KV heads) |
| 📐 Embedding Dimension | 512 |
| 🔁 FFN Activation | SwiGLU |
| 📍 Positional Encoding | RoPE |
| 🔧 Normalization | RMSNorm (pre-norm) |
| 💾 Training Hardware | 32GB GPU (CPU also works) |
| 👤 Creator | Skynetfc |

</div>

---

## Project Structure

```
meeru-by-skynetfc/
│
├── 📄 main.py                     ← Single entry point — run everything from here
├── ⚙️  config.yaml                 ← All hyperparameters in one place
├── 📋 requirements.txt
│
├── meeru/                         ← v1: Original monolithic implementation
│   ├── tokenizer.py               — BPE tokenizer (pure Python)
│   ├── model.py                   — Transformer architecture
│   ├── data_pipeline.py           — Wikipedia/arXiv/Reddit/GitHub/Gutenberg
│   ├── trainer.py                 — Training loop + checkpointing
│   ├── reasoning.py               — 4-stage reasoning engine
│   └── chat.py                    — Rich terminal chat interface
│
└── Meeru/                         ← v2: Modular production architecture
    ├── model/
    │   ├── transformer.py         — Full model assembly
    │   ├── attention.py           — MHA + GQA + RoPE + KV cache
    │   ├── embeddings.py          — Token + RoPE positional embeddings
    │   ├── feedforward.py         — GELU FFN and SwiGLU
    │   ├── normalization.py       — LayerNorm and RMSNorm
    │   └── moe.py                 — Mixture of Experts router
    │
    ├── training/
    │   ├── trainer.py             — DDP-aware training loop
    │   ├── optimizer.py           — AdamW + cosine LR schedule
    │   ├── loss.py                — CE + z-loss + MoE aux loss
    │   ├── checkpoint.py          — Save/resume with rotation
    │   ├── distributed.py         — Multi-GPU DDP
    │   └── monitor.py             — JSONL + wandb logging
    │
    ├── data/
    │   ├── collector.py           — All source orchestration
    │   ├── cleaner.py             — Quality filter + PII removal
    │   ├── deduplicator.py        — SHA-256 + MinHash LSH near-dedup
    │   ├── loader.py              — DDP-sharded batch loader
    │   └── tokenizer/
    │       └── bpe.py             — Canonical BPE tokenizer
    │
    ├── alignment/
    │   ├── reward_model.py        — Bradley-Terry reward model
    │   ├── rlhf.py                — PPO with KL penalty
    │   ├── constitution.py        — 6 constitutional principles
    │   └── red_team.py            — Adversarial test suite
    │
    ├── inference/
    │   ├── engine.py              — High-level generation interface
    │   ├── sampling.py            — Temperature, top-k, top-p, min-p
    │   ├── cache.py               — KV cache + sliding window
    │   └── reasoning.py           — CoT, self-consistency, tree-of-thought
    │
    ├── eval/
    │   ├── benchmarks.py          — Perplexity, multiple-choice, BoolQ
    │   ├── human_eval.py          — Structured human rating sessions
    │   └── safety.py              — Regex safety scanner
    │
    ├── serving/
    │   ├── api.py                 — Flask REST API
    │   ├── load_balancer.py       — Multi-strategy load balancer
    │   └── rate_limiter.py        — Token bucket rate limiter
    │
    └── config/
        └── model_config.yaml      — Full v2 configuration
```

---

## Quick Start

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

> GPU training (recommended): make sure CUDA is available. CPU training also works — just slower.

### 2. Check your setup

```bash
python main.py info
```

This shows hardware, model config, and which pipeline stages are complete.

### 3. Collect training data

```bash
python main.py collect
```

Fetches from Wikipedia, arXiv, Reddit, GitHub, and Project Gutenberg via public APIs.
No API keys required. A GitHub token speeds up collection — see [how-to-get-apis.md](how-to-get-apis.md).

Expected output:
```
[Meeru Data] Wikipedia : 45,231,847 chars
[Meeru Data] arXiv     :  8,142,209 chars
[Meeru Data] Reddit    :  6,834,012 chars
[Meeru Data] GitHub    : 12,903,441 chars
[Meeru Data] Gutenberg : 89,421,038 chars
```

### 4. Train the tokenizer

```bash
python main.py tokenize
```

Runs the BPE algorithm from scratch on your collected corpus. Learns 16,000 merge rules.
Saves to `data/tokenizer.json`. Takes ~5–15 minutes depending on corpus size.

### 5. Train the model

```bash
python main.py train
```

Starts training from random weights. Prints loss every 100 steps, evaluates every 1,000 steps, saves checkpoints automatically. **Training can be interrupted and resumed** — just run the same command again.

Expected training time:
| Hardware | Estimated time for 100k steps |
|---|---|
| CPU only (16GB RAM) | 3–7 days |
| Single GPU (8GB VRAM) | 18–36 hours |
| Single GPU (32GB VRAM, mixed precision) | 8–16 hours |
| Multi-GPU (DDP) | Scales linearly |

### 6. Chat with Meeru

```bash
python main.py chat
```

Loads the best checkpoint and opens the terminal chat interface.

```
  __  __  ___  ___  ____  _  _
 |  \/  || __|| __||  _ \| || |
 | |\/| || _| | _| | |_) | \/ |
 |_|  |_||___||___||____/|____|

by Skynetfc  ·  85M parameters  ·  1024-token context

You: What is a transformer neural network?
Meeru: A transformer is a neural network architecture that processes sequences
using a mechanism called self-attention...
```

### Run all stages sequentially

```bash
python main.py all
```

---

## Chat Interface Commands

| Command | What it does |
|---|---|
| `/reason` | Toggle step-by-step reasoning mode (chain-of-thought) |
| `/clear` | Reset conversation history |
| `/stats` | Show model info, generation parameters, device |
| `/exit` | Quit |

**Reasoning mode** shows Meeru's full thought process:
```
── Reasoning Chain ──
Step 1: What is the core concept?
  Neural networks are computational graphs...   Confidence: 72%

Step 2: What are the key mechanisms?
  Self-attention computes relationships...       Confidence: 81%

── Synthesis ──
Transformers are sequence models that...
Overall confidence: 76%
```

---

## Architecture

```
Input text
    │
    ▼
┌─────────────────────────────────────┐
│         BPE Tokenizer               │  16,000 merge rules
│    "hello world" → [142, 891]       │  learned from corpus
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│      Token Embeddings (512d)        │
│   + RoPE Positional Encodings       │  Relative positions
└─────────────────────────────────────┘
    │
    ▼  ×12 blocks
┌─────────────────────────────────────┐
│  ┌───────────────────────────────┐  │
│  │  RMSNorm                      │  │  Pre-norm for stability
│  │  ↓                            │  │
│  │  GQA Self-Attention           │  │  16 Q heads, 8 KV heads
│  │  ├─ Q, K, V projections       │  │  Causal mask (no peeking)
│  │  ├─ Scaled dot-product        │  │  KV cache at inference
│  │  └─ Output projection         │  │
│  │  ↓ + Residual                 │  │
│  │  RMSNorm                      │  │
│  │  ↓                            │  │
│  │  SwiGLU FFN (512→2048→512)    │  │  Gated activation
│  │  ↓ + Residual                 │  │
│  └───────────────────────────────┘  │
└─────────────────────────────────────┘
    │
    ▼
┌─────────────────────────────────────┐
│  Final RMSNorm                      │
│  Linear → 16,000 logits             │  Weight-tied to embeddings
└─────────────────────────────────────┘
    │
    ▼
 Next token probabilities
```

---

## Training Details

```
Optimizer  : AdamW (β₁=0.9, β₂=0.95, ε=1e-8)
Schedule   : Linear warmup (1k steps) → Cosine decay to 3e-5
LR range   : 3e-4 → 3e-5
Weight dec : 0.1 (matrices only — biases and norms excluded)
Grad clip  : 1.0
Batch size : 16 sequences × 1024 tokens
Grad accum : 4 → effective 65,536 tokens/step
Mixed prec : float16 forward + float32 optimizer state
Max steps  : 100,000 (~6.5B tokens total)
Eval every : 1,000 steps (50 validation batches)
Checkpoint : Every 1,000 steps (keeps last 3 + best)
```

---

## Data Sources

| Source | Content type | Volume | Key |
|---|---|---|---|
| **Wikipedia** | Encyclopedic articles | 40+ topics × 30 articles | None |
| **arXiv** | Research paper abstracts | 20 categories × 50 papers | None |
| **Reddit** | Q&A, discussion threads | 25 subreddits × 100 posts | None |
| **GitHub** | Project READMEs, docs | 20 topics × 25 repos | Optional ([get one](how-to-get-apis.md)) |
| **Project Gutenberg** | Public domain books | 200 full books | None |

**Total target:** ~500MB–1GB of clean deduplicated text

### Data cleaning pipeline

```
Raw text
  │
  ├─ Strip HTML markup + wiki syntax
  ├─ Remove URLs, emails, phone numbers
  ├─ Drop lines < 20 chars or < 40% alphabetic
  ├─ SHA-256 exact deduplication
  ├─ MinHash LSH near-duplicate removal (>80% similar)
  ├─ Paragraph-level dedup
  └─ Paragraph-level shuffle (prevents source ordering bias)
```

---

## Alignment & Safety

Meeru v2 (`Meeru/`) includes a full alignment stack:

### Constitutional AI
Six explicit principles that every response is checked against:
- Harmlessness — no instructions for harmful activities
- Honesty — acknowledge uncertainty, no fabrication
- Helpfulness — actually answer the question
- No bias — no unfair stereotypes
- Privacy — no PII handling
- No manipulation — no psychological exploitation

### RLHF
A complete PPO-based RLHF loop:
1. **Reward model** — trained on (prompt, chosen, rejected) preference triples
2. **Reference model** — frozen copy of initial policy for KL anchoring
3. **PPO updates** — clipped surrogate objective with KL penalty

### Red Team Suite
Automated adversarial testing across 8 built-in categories:
harmful content · jailbreaks · hallucination · bias · privacy · over-refusal

---

## REST API (v2)

Start the server:
```python
from Meeru.inference.engine import MeeruEngine
from Meeru.serving.api import run_server

engine = MeeruEngine()
run_server(engine, host="0.0.0.0", port=8080)
```

Endpoints:

**Generate text**
```bash
curl -X POST http://localhost:8080/generate \
  -H "Content-Type: application/json" \
  -d '{"prompt": "Explain attention mechanisms", "max_tokens": 256}'
```

**Multi-turn chat**
```bash
curl -X POST http://localhost:8080/chat \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [
      {"role": "user", "content": "What is a neural network?"},
      {"role": "assistant", "content": "A neural network is..."},
      {"role": "user", "content": "How does training work?"}
    ]
  }'
```

**Health check**
```bash
curl http://localhost:8080/health
```

**Rate limiting:** 20 request burst, 5 req/sec sustained, per client IP.
**Safety filtering:** Constitutional checker + regex scanner on every response.

---

## Optional Extras

### Enable Mixture of Experts

In `Meeru/config/model_config.yaml`:
```yaml
model:
  use_moe: true
  moe_every_n: 2         # MoE in every 2nd block
  moe_num_experts: 8     # 8 experts per MoE layer
  moe_top_k: 2           # each token activates 2 experts
```

With 8 experts and top-2 routing: same compute cost as dense, but **4× the effective parameter capacity**.

### Multi-GPU training

```bash
torchrun --nproc_per_node=4 -m Meeru.training.trainer
```

DDP is automatic — each GPU gets a non-overlapping data shard.

### Enable training dashboards (wandb)

```yaml
training:
  use_wandb: true
```

See [how-to-get-apis.md](how-to-get-apis.md) for setup.

---

## Frequently Asked Questions

**Q: Can I run this without a GPU?**
Yes. Training will be significantly slower but works on CPU. For a quick test, reduce `max_steps` to 5,000 in `config.yaml`.

**Q: Where does `main.py` save checkpoints?**
`checkpoints/meeru_best.pt` (best validation loss) and `checkpoints/meeru_latest.pt` (most recent). The last 3 step checkpoints are also kept.

**Q: Can I resume training if it's interrupted?**
Yes. Run `python main.py train` again — it automatically detects and resumes from the latest checkpoint.

**Q: What's the difference between `meeru/` and `Meeru/`?**
`meeru/` (lowercase) is version 1 — a single-file-per-component monolithic implementation used by `main.py`. `Meeru/` (uppercase) is version 2 — a fully modular production architecture with MoE, RLHF, GQA, RoPE, a REST API, and a serving layer. Both are complete, both work. Think of `meeru/` as v1 and `Meeru/` as v2.

**Q: How do I get an API key for the data sources?**
Most don't need one. See [how-to-get-apis.md](how-to-get-apis.md) for the full guide.

---

## Roadmap

- [x] BPE tokenizer from scratch
- [x] Decoder-only transformer architecture
- [x] Data collection pipeline (5 sources)
- [x] Training loop with AMP + gradient accumulation
- [x] Terminal chat interface
- [x] Modular v2 architecture (`Meeru/`)
- [x] RoPE + GQA + SwiGLU + RMSNorm
- [x] Mixture of Experts (optional)
- [x] KV cache for fast inference
- [x] Chain-of-thought / self-consistency / tree-of-thought
- [x] Constitutional AI + RLHF + PPO
- [x] Red team test suite
- [x] REST API with rate limiting
- [x] Multi-GPU DDP training
- [ ] Quantization (INT8/INT4 for deployment)
- [ ] Speculative decoding
- [ ] Streaming API responses
- [ ] Web chat UI

---

## API Keys Reference

No API keys are required to run Meeru. Optional keys improve data collection speed and add monitoring.

→ **Full guide:** [how-to-get-apis.md](how-to-get-apis.md)

---

## License

MIT License — see `LICENSE` for details.

---

<div align="center">

**Meeru** — built from scratch by **Skynetfc**

*No borrowed weights. No external AI APIs. Every line of intelligence was earned.*

```
"The model that trained itself into existence."
```

</div>
