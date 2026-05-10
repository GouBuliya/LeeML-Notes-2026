<div align="center">

[简体中文](./README.md) · [English](./README.en.md)

<img src="https://socialify.git.ci/GouBuliya/LeeML-Notes-2026/image?description=1&descriptionEditable=Hung-yi%20Lee%202026%20ML%2FDL%20course%20%E2%80%94%20110%20chapters%20of%20progressive%20notes%20with%204700%2B%20slide%20screenshots&font=Inter&forks=1&issues=0&language=1&name=1&owner=1&pattern=Charlie%20Brown&stargazers=1&theme=Auto" alt="LeeML-Notes-2026" width="720" />

# 🧠 LeeML-Notes-2026

### Hung-yi Lee's most-loved 2026 ML/DL course, broken into 110 weekly-readable chapters

[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC--BY--NC--SA%204.0-lightgrey.svg?style=flat-square)](./LICENSE)
[![Stars](https://img.shields.io/github/stars/GouBuliya/LeeML-Notes-2026?style=flat-square&color=yellow)](https://github.com/GouBuliya/LeeML-Notes-2026/stargazers)
[![Forks](https://img.shields.io/github/forks/GouBuliya/LeeML-Notes-2026?style=flat-square&color=blue)](https://github.com/GouBuliya/LeeML-Notes-2026/network)
[![Last Commit](https://img.shields.io/github/last-commit/GouBuliya/LeeML-Notes-2026?style=flat-square&color=green)](https://github.com/GouBuliya/LeeML-Notes-2026/commits)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](#-contributing)

📖 **110 chapters** &nbsp;·&nbsp; 🖼️ **4700+ slide screenshots** &nbsp;·&nbsp; 🚀 **PyTorch / Transformer / GAN / BERT / RL** &nbsp;·&nbsp; 🎯 **From zero to LLMs**

</div>

> [!IMPORTANT]
> **Notes are written in Simplified Chinese.** This repo is the most polished open-source companion to Prof. Hung-yi Lee's 2026 ML course (NTU). If you can read Chinese — or you're happy with auto-translation in your browser — you'll find this the cleanest, most navigable version of his notes anywhere on GitHub.

---

## ✨ Why this repo?

Prof. Hung-yi Lee's lectures are widely regarded as **the best Chinese-language ML course in the world**. The problem: 14+ hours of video, no chapter index, slides scattered across PDFs. This repo solves that.

| Pain point | What this repo gives you |
|---|---|
| 🥲 14h of video, no clear entry point | ✅ 110 progressive chapters, ordered from "what is ML" → Transformer → LLMs → RL → meta-learning |
| 🥲 Slide screenshots without context | ✅ Plain-language explanation of every slide, with formulas + code + figures |
| 🥲 Image links break offline | ✅ All 4700+ figures are local — clone once, read forever |
| 🥲 No idea what to skip | ✅ Three suggested learning paths: 7-day fastlane / 30-day systematic / 60-day research |
| 🥲 No way to verify learning | ✅ All 14 official assignments included with Colab notebook references |

---

## 🗺️ Learning roadmap

```mermaid
flowchart TD
    Start([🚀 Start]) --> M1[1. Fundamentals<br/>Intro · PyTorch · Backprop]
    M1 --> M2[2. Training in practice<br/>Optimizers · Batch · Overfit]
    M2 --> M3[3. Vision & Sequence<br/>CNN · RNN · GNN · Self-Attention]
    M3 --> M4[4. Transformer<br/>Encoder-Decoder · Pointer Network]
    M4 --> M5[5. Generative Models<br/>GAN · VAE · Flow]
    M4 --> M6[6. The LLM Era<br/>BERT · GPT · Self-supervised]
    M5 --> M7[7. Representation & XAI<br/>Auto-encoder · t-SNE · Adversarial]
    M6 --> M7
    M7 --> M8[8. Reinforcement Learning<br/>Policy Gradient · Q-learning]
    M7 --> M9[9. Model Compression<br/>Pruning · Distillation · Quantization]
    M8 --> M10[10. Lifelong & Meta Learning<br/>MAML · Meta-RL]
    M9 --> M10
    M10 --> Done([🏆 Done])

    style Start fill:#FFD700,stroke:#333,stroke-width:2px
    style Done fill:#90EE90,stroke:#333,stroke-width:2px
    style M4 fill:#FF6B6B,stroke:#333,color:#fff
    style M6 fill:#4ECDC4,stroke:#333,color:#fff
```

---

## 📚 Eight modules at a glance

| # | Module | Lectures | Highlights |
|---|---|---|---|
| 1 | 🎯 **Fundamentals** | 1-12 | Intro · PyTorch · Backprop · Logistic Regression |
| 2 | ⚡ **Training in practice** | 13-23 | Optimizers · Adam · Batch & Momentum · Overfitting |
| 3 | 🖼️ **Vision & Sequence** | 24-35 | CNN · RNN · GNN · Self-Attention |
| 4 | 🤖 **Transformer** | 36-41 | Encoder · Decoder · Pointer · Non-Autoregressive |
| 5 | 🎨 **Generative Models** | 42-51 | GAN · VAE · Flow-based |
| 6 | 🧬 **BERT / GPT / Self-supervised** | 52-59 | BERT pre-training · GPT-3 · Variants |
| 7 | 🔍 **Representation · XAI · Adversarial** | 61-75 | Auto-encoder · t-SNE · Domain Adaptation · Attacks |
| 8 | 🎮 **RL · Compression · Meta** | 76-110 | Policy Gradient · Q-Learning · Pruning · MAML |

> Browse the [Chinese README](./README.md) for the curated chapter highlights and full chapter list.

---

## 🛤️ Three learning paths

| Path | For | Time | Route |
|---|---|---|---|
| 🚀 **7-day fastlane** | Already know Python + linear algebra, want LLM intuition fast | 1 week / 1.5h per day | Modules 1 → 2 → 4 → 6 |
| 📚 **30-day systematic** | Junior CS student / new ML engineer | 1 month / 1h per day | All 8 modules in order, all assignments |
| 🎓 **60-day research** | Preparing for grad school / publishing | 2 months / 1.5h per day | All 110 chapters + recommended papers + Colab reproductions |

---

## 🛠️ How to read

| Setting | Recommended | Notes |
|---|---|---|
| 📱 Casual reading | **GitHub web** | Math + figures render natively |
| 💻 Deep study | **VSCode + Markdown All in One** | Full-text search across all chapters |
| 🧩 Knowledge graph | **Obsidian / Logseq** | Open the cloned folder as a vault |
| 🖨️ Print | **Pandoc → PDF** | `pandoc *.md -o leeml.pdf --pdf-engine=xelatex` |

```bash
# Clone offline (~196 MB with all hi-res slides)
git clone https://github.com/GouBuliya/LeeML-Notes-2026.git
cd LeeML-Notes-2026
```

---

## 🌟 Companion resources

- 🎬 **Course videos** — [Prof. Hung-yi Lee on YouTube](https://www.youtube.com/c/HungyiLeeNTU)
- 🎞️ **Official slides** — [speech.ee.ntu.edu.tw/~hylee](https://speech.ee.ntu.edu.tw/~hylee/index.html)
- 💻 **Assignment Colabs** — referenced inside each "Homework" chapter
- 📦 **Pair with** — [pytorch/examples](https://github.com/pytorch/examples) for hands-on code

---

## 🤝 Contributing

PRs welcome — even for typos.

- 🐛 Found something wrong? Open an [Issue](https://github.com/GouBuliya/LeeML-Notes-2026/issues/new)
- ✍️ Want to add content? See [CONTRIBUTING.md](./CONTRIBUTING.md)
- 💬 Got a question? Use [Discussions](https://github.com/GouBuliya/LeeML-Notes-2026/discussions)
- ⭐ The simplest way to support: **drop a Star**

---

## 📈 Star History

<a href="https://star-history.com/#GouBuliya/LeeML-Notes-2026&Date">
  <img src="https://api.star-history.com/svg?repos=GouBuliya/LeeML-Notes-2026&type=Date" alt="Star History Chart" width="600"/>
</a>

---

## 🙏 Acknowledgements

This repo organizes and reformats Chinese notes already in the public domain. All credit goes to:

- **Prof. Hung-yi Lee** (National Taiwan University) — original course author. All slides, figures, and intellectual content belong to him.
- **龙哥盟 (Dragon League)** at [cnblogs.com/geekdoc](https://www.cnblogs.com/geekdoc) — original Chinese transcription.
- **OpenDocCN** ([@OpenDocCN](https://github.com/OpenDocCN)) — image mirror.

> This repo only adds **chapter splitting + formatting cleanup + image localization**. It claims no rights over the underlying content. If the original author has any concerns, please open an Issue and I'll act immediately.

---

## 📜 License

- **Repository structure & formatting** — [CC BY-NC-SA 4.0](./LICENSE) (Attribution · Non-Commercial · Share-Alike)
- **Course content (text / figures / equations)** — © Prof. Hung-yi Lee & National Taiwan University, included under educational fair use
- **Commercial use prohibited** (no paid courses, paid communities, or resale)

---

<div align="center">

**If this saved you hours, please drop a ⭐ — that's the biggest encouragement I can ask for**

<sub>Made with ❤️ by ML learners, for ML learners</sub>

</div>
