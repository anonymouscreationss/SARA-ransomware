<p align="center">
  <img src="assets/jrok_logo.png" alt="JROK" width="480"/>
</p>

<p align="center">
  <strong>An independent AI language model + professional cybersecurity lab — built entirely from scratch.</strong><br/>
  <em>No external AI APIs. No borrowed weights. No shortcuts.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Built%20by-Skynetfc-00ff88?style=for-the-badge&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZmlsbD0iI2ZmZiIgZD0iTTEyIDJMMiA3bDEwIDUgMTAtNS0xMC01ek0yIDE3bDEwIDUgMTAtNS0xMC01LTEwIDV6TTIgMTJsMTAgNSAxMC01LTEwLTUtMTAgNXoiLz48L3N2Zz4=" alt="Built by Skynetfc"/>
  <img src="https://img.shields.io/badge/Python-3.10%2B-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python 3.10+"/>
  <img src="https://img.shields.io/badge/PyTorch-2.0%2B-EE4C2C?style=for-the-badge&logo=pytorch&logoColor=white" alt="PyTorch"/>
  <img src="https://img.shields.io/badge/Parameters-~85M-00ff88?style=for-the-badge" alt="~85M Parameters"/>
  <img src="https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge" alt="MIT License"/>
</p>

<p align="center">
  <a href="#-what-is-jrok">What is JROK?</a> •
  <a href="#-architecture">Architecture</a> •
  <a href="#-quick-start">Quick Start</a> •
  <a href="#-security-lab">Security Lab</a> •
  <a href="#-project-structure">Project Structure</a> •
  <a href="#-roadmap">Roadmap</a>
</p>

---

## ⚡ What is JROK?

**JROK** is two things in one repository:

| Module | Description |
|---|---|
| 🧠 **JROK AI** | A decoder-only transformer language model (~85M params) trained from scratch |
| 🔐 **JROK Security Lab** | A full professional cybersecurity lab: Red, Blue, Gray, White Teams + Crypto + Forensics |

Every single line of code was written from first principles by **Skynetfc**. There are no API wrappers around ChatGPT, no fine-tuned Llama weights, no borrowed security scanners. JROK is 100% original.

> **Why build from scratch?**
> Because understanding a system deeply — down to every weight, every byte, every exploit — is the only way to truly own it.

---

## 🧠 JROK AI

### Architecture

JROK uses a **decoder-only transformer** architecture with modern improvements:

| Component | Specification |
|---|---|
| Architecture | Decoder-only Transformer |
| Parameters | ~85 million |
| Context window | 1,024 tokens |
| Vocabulary | 16,000 BPE tokens |
| Embedding dim | 512 |
| Attention heads | 16 |
| Layers | 12 transformer blocks |
| FFN hidden dim | 2,048 (4× embedding) |
| Positional encoding | Learned |
| Normalization | Pre-LayerNorm |
| Training precision | AMP (FP16/BF16) |

### JROK v2 (`Jrok/`) — Production Architecture

The `Jrok/` directory contains a modular v2 design with production-grade features:

- **Grouped Query Attention (GQA)** — reduces KV-cache memory by 4×
- **RoPE (Rotary Position Embeddings)** — better length generalization
- **RMSNorm** — faster, simpler normalization
- **SwiGLU activations** — improved FFN performance
- **Mixture of Experts (MoE)** — sparse computation for scaling
- **Full RLHF / Constitutional AI alignment stack**
- **Production serving** — load balancer, rate limiter, REST API

### Training Pipeline

```
Public Data Sources          Corpus (~500M chars)         Trained Model
─────────────────────        ──────────────────────       ──────────────
Wikipedia      ──┐
arXiv          ──┤  collect  →  corpus.txt  →  tokenize  →  BPE vocab
Reddit         ──┤                                            16K tokens
GitHub         ──┤                                               │
Project        ──┘                                               ↓
Gutenberg                                                    train
                                                          100K steps
                                                          AMP + cosine LR
                                                               │
                                                               ↓
                                                           jrok_best.pt
                                                               │
                                                               ↓
                                                            chat
                                                        Rich terminal UI
```

### Reasoning Engine

JROK includes a **4-stage Chain-of-Thought reasoning engine**:

1. **Decompose** — break the problem into sub-questions
2. **Research** — retrieve relevant knowledge from context
3. **Synthesize** — combine evidence into a coherent answer
4. **Verify** — check consistency and confidence before responding

---

## 🚀 Quick Start

### Prerequisites

```bash
# Python 3.10 or higher required
python --version

# Install dependencies
pip install -r requirements.txt
```

### 1. Collect Training Data

```bash
python main.py collect
```

Fetches ~500M characters of text from Wikipedia, arXiv, Reddit, GitHub, and Project Gutenberg using only free public APIs. No keys required (optional GitHub token improves rate limits).

### 2. Train the Tokenizer

```bash
python main.py tokenize
```

Trains a Byte-Pair Encoding (BPE) tokenizer from scratch on the collected corpus. Learns 16,000 merge rules. No SentencePiece, no tiktoken — pure Python.

### 3. Train the Model

```bash
python main.py train
```

Trains the 85M parameter transformer from random weights. Supports:
- Automatic Mixed Precision (AMP) for GPU efficiency
- Gradient accumulation (effective batch = 64 sequences)
- Cosine learning rate schedule with warmup
- Automatic checkpointing every 1,000 steps
- Resume from checkpoint (`--resume` is on by default)

### 4. Chat

```bash
python main.py chat
```

Opens a Rich terminal chat interface. Commands inside chat:
- `/reasoning on` — show full chain-of-thought
- `/temp 0.7` — set temperature
- `/reset` — clear conversation history
- `/info` — show model stats
- `quit` — exit

### 5. System Info

```bash
python main.py info
```

Shows hardware, model configuration, estimated parameter count, and file status.

---

## 🔐 Security Lab

JROK includes a **complete, professional-grade cybersecurity lab** with 6 teams. All tools use only real code — no mocked data, no simulated results.

```bash
python main.py security [team] [tool] [options]
```

> ⚠️ **Legal Warning:** These tools are for authorized testing only. Only use against systems you own or have explicit written permission to test. Unauthorized use is illegal.

---

### 🔴 Red Team — Offensive Security

```bash
python main.py security red recon     --target example.com
python main.py security red scan      --target 192.168.1.0/24 --ports 1-1024
python main.py security red web       --target https://example.com
python main.py security red password  --hash "$2b$12$..." --wordlist wordlists/passwords_top1000.txt
python main.py security red osint     --target example.com
python main.py security red payload   --type reverse_shell --lang python --lhost 10.0.0.1 --lport 4444
python main.py security red network   --target 192.168.1.0/24
python main.py security red report    --findings findings.json
```

| Tool | Capability |
|---|---|
| `recon.py` | DNS enumeration, subdomain brute-force, TLS inspection, email harvesting |
| `scanner.py` | TCP/UDP port scan, banner grabbing, OS fingerprinting |
| `web_tester.py` | SQLi (error/boolean/time-based), XSS, LFI, directory fuzzing |
| `password_audit.py` | Hash identification, dictionary crack, strength scoring |
| `osint.py` | WHOIS, CT logs, IP geolocation, GitHub secret scan, 40+ platform checks |
| `network_tester.py` | ARP scan, protocol checks (FTP/SMTP/SNMP/LDAP/Redis/MongoDB) |
| `payload_gen.py` | Reverse shells in 8 languages, web shells, encoders |
| `report.py` | JSON + Markdown findings with CVSS-style scoring |

---

### 🔵 Blue Team — Defensive Security

```bash
python main.py security blue monitor  --interval 5
python main.py security blue ids      --watch --rules default
python main.py security blue siem     --dashboard
python main.py security blue honeypot --ssh --http --ftp
python main.py security blue fim      --watch /etc --baseline
python main.py security blue incident --playbook ransomware
```

| Tool | Capability |
|---|---|
| `monitor.py` | Real-time process/network/filesystem/resource monitoring |
| `ids.py` | 15-rule signature IDS + anomaly detector |
| `siem.py` | Full correlation engine, 20 stateful detection rules, MITRE-tagged alerts |
| `network_monitor.py` | `/proc/net/tcp\|udp` tracking, ARP monitor, TOR exit detection |
| `honeypot.py` | SSH/HTTP/FTP honeypots with canary file support |
| `file_integrity.py` | HMAC-chained FIM with watch mode |
| `log_analyzer.py` | Apache/Nginx/auth/syslog log parser |
| `threat_intel.py` | IOC store, STIX export, VT/AbuseIPDB integration |
| `incident.py` | Full IR lifecycle with 5 response playbooks |

---

### 🟣 Gray Team — Vulnerability Research

```bash
python main.py security gray vuln     --target 192.168.1.1 --ports 22,80,443
python main.py security gray purple   --technique T1059 --simulate
python main.py security gray threat   --asset "Web App" --model stride
python main.py security gray exploit  --search "apache log4j"
python main.py security gray risk     demo
```

| Tool | Capability |
|---|---|
| `vuln_scanner.py` | NVD CVE lookup, TLS checks, port risk mapping |
| `purple_exercise.py` | 12 ATT&CK techniques, gap analysis |
| `threat_model.py` | STRIDE + DREAD threat modeling |
| `exploit_db.py` | NVD CVE search, CISA KEV catalog, GHSA, attack chain builder |
| `risk_matrix.py` | Full risk assessment: assets × threats × controls → heat map + Monte Carlo |

---

### ⚪ White Team — Governance & Compliance

```bash
python main.py security white policy    generate POL-001 --org "JROK"
python main.py security white policy    all --org "JROK"
python main.py security white report    board
python main.py security white report    monthly
python main.py security white audit     --log audit.jsonl --verify
python main.py security white scoring   --leaderboard
python main.py security white roe       --generate --target "192.168.1.0/24"
```

| Tool | Capability |
|---|---|
| `policy.py` | 15 security policies (ISO 27001-aligned, HMAC-signed) |
| `executive_report.py` | Monthly briefing, quarterly board report, compliance posture |
| `compliance.py` | CIS Linux, NIST 800-53, OWASP, PCI DSS checklists |
| `audit.py` | HMAC-chained tamper-evident audit trail |
| `roe.py` | Signed Rules of Engagement documents |
| `scoring.py` | Red/Blue team scoring and leaderboard |

---

### 🔑 Crypto Module

```bash
python main.py security crypto hasher  sha256 --file data.bin
python main.py security crypto cipher  encrypt --alg aes256gcm --in plain.txt --out enc.bin
python main.py security crypto pki     create-ca --name "JROK Root CA"
python main.py security crypto cert    inspect --host github.com
python main.py security crypto stego   hide input.bmp secret.txt output.bmp
python main.py security crypto stego   analyze suspicious.bmp
```

| Tool | Capability |
|---|---|
| `hasher.py` | Multi-algo hashing, HMAC, PBKDF2, scrypt, Merkle tree |
| `cipher.py` | AES-256-GCM/CBC, ChaCha20-Poly1305, RSA-4096 |
| `cert_inspector.py` | Live TLS fetch, CT log lookup, PEM parse |
| `pki.py` | Full PKI: Root CA → Intermediate → Server cert, mTLS, CRL, PKCS#12 |
| `steganography.py` | LSB stego in BMP/WAV/text, AES-encrypted payloads, chi-squared steganalysis |

---

### 🔬 Forensics Module

```bash
python main.py security forensics disk     --image /dev/sdb --output case_001/
python main.py security forensics memory   --pid 1234
python main.py security forensics timeline --fs /mnt/evidence --auth /var/log/auth.log
python main.py security forensics malware  --file suspicious.exe
python main.py security forensics network  analyze capture.pcap
python main.py security forensics registry hive NTUSER.DAT
```

| Tool | Capability |
|---|---|
| `disk_analyzer.py` | File carving (magic bytes), entropy analysis, PE header parser, chain of custody |
| `memory_analyzer.py` | Live `/proc/PID/maps` + `/proc/PID/mem` analysis, shellcode detection |
| `timeline.py` | Multi-source forensic timeline (FS MAC times, auth log, shell history, browser SQLite) |
| `malware_classifier.py` | YARA-lite engine (14 rules), API behavior fingerprinting, packer detection, MITRE mapping |
| `network_forensics.py` | Pure-Python PCAP parser, TCP stream reassembly, DNS/HTTP/TLS analysis, C2 beacon detection |
| `registry_analyzer.py` | Windows registry hive parser (REGF binary + .reg text), persistence detection, UserAssist decode |

---

## 📁 Project Structure

```
jrok/
│
├── main.py                        ← Unified entry point for AI + Security
├── config.yaml                    ← All configuration (AI + Security Lab)
├── requirements.txt               ← Python dependencies
├── assets/
│   └── jrok_logo.png              ← JROK brand logo
│
├── jrok/                          ← AI Core v1 (monolithic, production-ready)
│   ├── tokenizer.py               ← BPE tokenizer from scratch
│   ├── model.py                   ← 85M transformer, prefill + decode
│   ├── data_pipeline.py           ← Wikipedia/arXiv/Reddit/GitHub/Gutenberg
│   ├── trainer.py                 ← AMP, gradient accum, cosine LR
│   ├── reasoning.py               ← 4-stage Chain-of-Thought
│   └── chat.py                    ← Rich terminal interface
│
├── Jrok/                          ← AI Core v2 (modular, production architecture)
│   ├── model/                     ← GQA, RoPE, RMSNorm, SwiGLU, MoE
│   ├── training/                  ← Trainer, optimizer, checkpointing, distributed
│   ├── data/                      ← Tokenizer, collector, cleaner, deduplicator
│   ├── alignment/                 ← Constitution AI, RLHF, reward model, red team
│   ├── inference/                 ← Engine, sampling strategies, KV cache
│   ├── eval/                      ← Benchmarks, HumanEval, safety tests
│   └── serving/                   ← REST API, load balancer, rate limiter
│
├── security/                      ← JROK Cybersecurity Lab
│   ├── lab.py                     ← Unified security CLI dispatcher
│   ├── config.yaml                ← Lab configuration
│   ├── README.md                  ← Security lab usage guide
│   │
│   ├── red_team/                  ← Offensive Security
│   │   ├── recon.py               ← DNS, subdomain, TLS, email harvest
│   │   ├── scanner.py             ← TCP/UDP scan, banner, OS fingerprint
│   │   ├── web_tester.py          ← SQLi, XSS, LFI, dir fuzz
│   │   ├── password_audit.py      ← Hash crack, strength scoring
│   │   ├── osint.py               ← WHOIS, CT logs, GitHub, 40+ platforms
│   │   ├── network_tester.py      ← ARP, protocol checks, pivot detection
│   │   ├── payload_gen.py         ← Shells in 8 languages, encoders
│   │   └── report.py              ← JSON + Markdown findings
│   │
│   ├── blue_team/                 ← Defensive Security
│   │   ├── monitor.py             ← Real-time system monitor
│   │   ├── ids.py                 ← 15-rule signature IDS + anomaly
│   │   ├── siem.py                ← Full SIEM: 20 rules, MITRE tags
│   │   ├── network_monitor.py     ← /proc/net connection tracking
│   │   ├── honeypot.py            ← SSH/HTTP/FTP honeypots
│   │   ├── file_integrity.py      ← HMAC-chained FIM
│   │   ├── log_analyzer.py        ← Apache/Nginx/auth/syslog
│   │   ├── threat_intel.py        ← IOC store, STIX, VT/AbuseIPDB
│   │   └── incident.py            ← IR lifecycle, 5 playbooks
│   │
│   ├── gray_team/                 ← Vulnerability Research
│   │   ├── vuln_scanner.py        ← NVD CVE lookup, TLS checks
│   │   ├── purple_exercise.py     ← 12 ATT&CK techniques
│   │   ├── threat_model.py        ← STRIDE + DREAD
│   │   ├── exploit_db.py          ← NVD, CISA KEV, GHSA, attack chains
│   │   └── risk_matrix.py         ← Full risk assessment + heat map
│   │
│   ├── white_team/                ← Governance & Compliance
│   │   ├── policy.py              ← 15 security policies (ISO 27001)
│   │   ├── executive_report.py    ← Board/CEO reports
│   │   ├── compliance.py          ← CIS, NIST, OWASP, PCI DSS
│   │   ├── audit.py               ← Tamper-evident audit trail
│   │   ├── roe.py                 ← Rules of Engagement docs
│   │   └── scoring.py             ← Red/Blue team scoring
│   │
│   ├── crypto/                    ← Cryptography Tools
│   │   ├── hasher.py              ← SHA/HMAC/PBKDF2/Merkle
│   │   ├── cipher.py              ← AES-256-GCM, ChaCha20, RSA-4096
│   │   ├── cert_inspector.py      ← Live TLS + CT log analysis
│   │   ├── pki.py                 ← Full PKI + mTLS + PKCS#12
│   │   └── steganography.py       ← LSB stego + chi-squared analysis
│   │
│   └── forensics/                 ← Digital Forensics
│       ├── disk_analyzer.py       ← File carving, PE headers, CoC
│       ├── memory_analyzer.py     ← Live /proc analysis, shellcode
│       ├── timeline.py            ← Multi-source forensic timeline
│       ├── malware_classifier.py  ← YARA-lite, behavior, MITRE
│       ├── network_forensics.py   ← PCAP parser, beacon detection
│       └── registry_analyzer.py   ← Windows registry forensics
│
├── wordlists/                     ← Security wordlists
│   ├── subdomains.txt             ← 400+ subdomain names
│   ├── common_dirs.txt            ← 350+ web paths/directories
│   └── passwords_top1000.txt      ← Top 1000 common passwords
│
├── guides/
│   ├── run-locally.md             ← Local setup guide
│   └── run-on-colab.md            ← Google Colab guide
│
└── data/                          ← Generated (not committed)
    ├── corpus.txt                 ← Collected training text
    └── tokenizer.json             ← Trained BPE vocabulary
```

---

## 🛠️ Requirements

```
torch>=2.0.0
pyyaml>=6.0
rich>=13.0.0
requests>=2.28.0
cryptography>=41.0.0
psutil>=5.9.0
```

Install everything:

```bash
pip install -r requirements.txt
```

> **GPU Training:** JROK trains on CPU but is designed for CUDA. A GPU with ≥8GB VRAM is recommended for the 85M model. Google Colab (free tier) works fine — see [`guides/run-on-colab.md`](guides/run-on-colab.md).

---

## ⚙️ Configuration

All settings live in `config.yaml`. Key sections:

```yaml
# AI model architecture
model:
  embedding_dim: 512        # increase to 768 for ~200M params
  num_heads: 16
  num_layers: 12
  context_length: 1024      # increase to 2048 with more VRAM
  vocab_size: 16000

# Training hyperparameters
training:
  batch_size: 16            # reduce to 4 if OOM
  learning_rate: 3.0e-4
  max_steps: 100000         # ~10 hours on RTX 3080

# Security lab
security_config:
  red_team:
    max_threads: 50
    port_scan_rate: 1000
  blue_team:
    monitor_interval: 5
```

---

## 📈 Scaling Guide

Want a bigger model? Change these values in `config.yaml`:

| Target | `embedding_dim` | `num_heads` | `num_layers` | Approx Params | VRAM |
|---|---|---|---|---|---|
| Tiny | 256 | 8 | 6 | ~12M | 2 GB |
| **Default** | **512** | **16** | **12** | **~85M** | **8 GB** |
| Medium | 768 | 12 | 24 | ~350M | 24 GB |
| Large | 1024 | 16 | 24 | ~600M | 40 GB |
| XL | 2048 | 16 | 32 | ~3B | 80 GB |

---

## 🗺️ Roadmap

### AI
- [x] BPE tokenizer from scratch
- [x] Transformer v1 (85M params)
- [x] Training loop (AMP, cosine LR, gradient accum)
- [x] 4-stage reasoning engine
- [x] Rich chat interface
- [x] v2 modular architecture (GQA, RoPE, RMSNorm, MoE)
- [x] Full RLHF alignment stack
- [x] Production serving (REST API, load balancer)
- [ ] Distributed training (DDP)
- [ ] Attention sink for long context (>4K tokens)
- [ ] Web UI (Gradio / Streamlit)
- [ ] Model export (ONNX / GGUF for llama.cpp)

### Security Lab
- [x] Red Team: recon, scanner, web, password, OSINT, payload, network
- [x] Blue Team: monitor, IDS, SIEM (20 rules), honeypot, FIM, log, IR
- [x] Gray Team: vuln scanner, purple exercise, threat model, exploit DB, risk matrix
- [x] White Team: 15 policies, board reports, compliance, audit, scoring
- [x] Crypto: hashing, AES/ChaCha20/RSA, PKI, steganography
- [x] Forensics: disk, memory, timeline, malware classifier, network PCAP, registry
- [ ] C2 framework (for authorized red team exercises)
- [ ] Active Directory attack paths (BloodHound-style, pure Python)
- [ ] Cloud security scanner (AWS/GCP/Azure misconfig)
- [ ] Mobile (APK static analysis)

---

## 🤝 Contributing

JROK is a personal project by Skynetfc. Contributions, bug reports, and ideas are welcome.

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

Please follow these principles:
- **No mocks** — all code must produce real results
- **No external AI APIs** — the AI module must stay fully independent
- **Comments** — explain the *why*, not the *what*
- **Security ethics** — all offensive tools must default to authorized-only behavior

---

## ⚖️ Legal & Ethics

### AI
JROK is trained exclusively on publicly available text. No proprietary or copyrighted training data is included. The training pipeline uses free public APIs only.

### Security Tools
The security tools in this repository are intended **solely for:**
- Authorized penetration testing of systems you own
- Academic and educational security research
- CTF (Capture the Flag) competitions
- Professional security assessments with written permission

**Using these tools against systems without authorization is illegal** under the Computer Fraud and Abuse Act (CFAA), the UK Computer Misuse Act, and equivalent laws in most jurisdictions. The author assumes no liability for misuse.

---

## 📄 License

```
MIT License

Copyright (c) 2026 Skynetfc

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
```

---

<p align="center">
  <img src="assets/jrok_logo.png" alt="JROK" width="200"/>
  <br/>
  <strong>JROK</strong> — Built from nothing, by Skynetfc.
  <br/>
  <em>Every weight earned. Every byte written. Every exploit understood.</em>
</p>
