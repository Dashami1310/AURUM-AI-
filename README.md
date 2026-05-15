<div align="center">

# 🎙️ AURUM AI
### Audio Language Model System

**An end-to-end multimodal AI pipeline that listens, understands emotion, reasons intelligently, and speaks back.**


</div>

---

## 📌 Overview

**AURUM AI** is a voice-first AI assistant built around a modular **Audio Language Model (ALM)** pipeline. It transcribes speech in real time, detects the speaker's emotional tone, reasons contextually using a quantized on-device LLM, and responds with synthesized voice — all within a single elegant interaction loop.

Built entirely on free-tier hardware (Google Colab T4 GPU) using open-source models, AURUM AI is an individual deep-dive into multimodal AI — demonstrating that voice-first intelligence is accessible without expensive infrastructure.

---

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────┐
│                      AURUM AI PIPELINE                  │
│                                                         │
│   🎤 Voice Input                                        │
│        │  (Manual JS Recorder — Browser-based)          │
│        ▼                                                │
│   ┌─────────────┐     ┌──────────────────┐             │
│   │ Whisper-Base│────►│  Transcription   │             │
│   └─────────────┘     └────────┬─────────┘             │
│                                │                        │
│   ┌─────────────┐     ┌────────▼─────────┐             │
│   │  wav2vec2   │────►│ Emotion Context  │             │
│   └─────────────┘     └────────┬─────────┘             │
│                                │                        │
│   ┌──────────────────┐ ┌───────▼──────────┐            │
│   │ Qwen2.5-1.5B     │ │  Reasoning &     │            │
│   │ (4-bit Quantized)│─│  Response Gen    │            │
│   └──────────────────┘ └────────┬─────────┘            │
│                                 │                       │
│   ┌─────────────┐     ┌─────────▼────────┐             │
│   │  Kokoro TTS │────►│  Voice Output    │             │
│   └─────────────┘     └──────────────────┘             │
│                                                         │
│   💾 SQLite  ─────────  Conversation Memory             │
└─────────────────────────────────────────────────────────┘
```

---

## ✨ Features

| Feature | Description |
|---|---|
| 🎤 **Voice Input** | Browser-based manual JS recorder — no mic permission hassles |
| 🔤 **Speech-to-Text** | Whisper-Base for accurate real-time transcription |
| 💬 **Emotion Detection** | wav2vec2 classifies speaker emotion; fed as LLM context |
| 🤖 **On-Device Reasoning** | Qwen2.5-1.5B-Instruct in 4-bit quantization — runs on free Colab GPU |
| 🔊 **Voice Response** | Kokoro TTS synthesizes replies with natural-sounding speech |
| 📜 **Memory** | SQLite stores full conversation history across a session |
| 🎨 **Stylish UI** | Gradio interface with a custom Black-Gold dark theme |

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| **Speech Recognition** | `openai/whisper-base` |
| **Emotion Classification** | `facebook/wav2vec2-base` |
| **Language Model** | `Qwen/Qwen2.5-1.5B-Instruct` |
| **Quantization** | `BitsAndBytes` — 4-bit NF4 |
| **Text-to-Speech** | `Kokoro TTS` |
| **UI Framework** | `Gradio` |
| **Persistence** | `SQLite` |
| **Platform** | Google Colab (T4 GPU) |
| **Language** | Python 3.10 |

---

## 🚀 Getting Started

### Prerequisites

- A **Google Colab** account (free tier works)
- **GPU Runtime**: T4 (Runtime → Change Runtime Type → T4 GPU)
- A **Hugging Face** account (for model access)

### Installation & Run

1. Click the **Open in Colab** badge at the top of this README
2. Set Runtime to **GPU → T4**
3. Run all cells from top to bottom
4. Models download automatically on first run (~5–10 mins)
5. The **Gradio UI** launches at the end — interact via the black-gold interface

> ⚠️ **Note:** Model weights (Whisper, wav2vec2, Qwen) are large and are downloaded at runtime. They are not stored in this repository.

### Key Dependencies

```bash
# Installed automatically inside the notebook
pip install transformers==4.40.0
pip install accelerate bitsandbytes
pip install openai-whisper
pip install gradio
pip install kokoro
pip install torch torchaudio
```

---

## 📁 Repository Structure

```
AURUM-AI/
│
├── AURUM_AI.ipynb          # Main Google Colab notebook (full pipeline)
├── requirements.txt        # Python dependencies
├── README.md               # Project documentation

```

---

## 🐛 Challenges Overcome

| Problem | Root Cause | Fix Applied |
|---|---|---|
| CUDA dtype mismatch | Mixed float32/float16 across models | Explicit `torch_dtype=torch.float16` on all models |
| Out-of-Memory (OOM) on T4 | Full-precision Qwen2.5 too large | 4-bit quantization via `BitsAndBytesConfig` |
| Browser mic not capturing audio | Colab JS sandbox restrictions | Built custom manual JS voice recorder widget |
| Gradio audio latency | Input component polling delays | Replaced with manual recorder + upload trigger |
| wav2vec2 input shape error | Resampling mismatch | Added `torchaudio.transforms.Resample` to 16kHz |

---

## 📊 Model Details

| Model | Size | Task | Quantization |
|---|---|---|---|
| `openai/whisper-base` | ~145 MB | Speech-to-Text | FP16 |
| `facebook/wav2vec2-base` | ~360 MB | Emotion Detection | FP32 |
| `Qwen/Qwen2.5-1.5B-Instruct` | ~1.5B params | Text Generation | 4-bit NF4 |
| `Kokoro TTS` | ~80 MB | Text-to-Speech | FP32 |

---

## 🎯 Use Cases

- 🧑‍💼 **Emotion-aware virtual assistants** that adapt tone based on how users feel
- 🎓 **Educational voice bots** for interactive Q&A
- 🏥 **Mental wellness check-ins** using voice emotion cues
- 🔬 **Research prototype** for multimodal ALM pipelines on constrained hardware

---

## 🔮 Future Roadmap

- [ ] Upgrade to Whisper-Medium for better accuracy
- [ ] Add multi-language support (Hindi, Kannada)
- [ ] Fine-tune Qwen on domain-specific dialogue
- [ ] Deploy to Hugging Face Spaces for public demo
- [ ] Real-time streaming TTS response

---

## 👩‍💻 Author

**Dashami**


---

<div align="center">

**⭐ If you found this useful, give it a star!**

</div>
