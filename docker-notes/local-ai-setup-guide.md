# 🐳 Running AI Locally with Docker Desktop
### A step-by-step guide to replacing GitHub Copilot with a free, private, GPU-accelerated local AI

---

## 🧠 Why Run AI Locally?

| Problem with Cloud AI | Local AI Solution |
|---|---|
| 💸 Costly at scale ($10–30/million tokens) | ✅ Completely free |
| 🌐 Needs internet to work | ✅ Works fully offline |
| 🔒 Your code goes to third-party servers | ✅ Everything stays on your machine |
| ⚡ Latency depends on network | ✅ GPU-accelerated, sub-300ms responses |

---

## 🖥️ System Requirements

> This guide is optimized for the following setup. Adjust model size based on your hardware.

| Component | Spec | Role in AI Inference |
|---|---|---|
| 💻 Laptop | Lenovo IdeaPad Pro 5 16IMH9 | — |
| 🧠 CPU | Intel Core Ultra 9 185H | Handles overflow inference |
| ⚡ NPU | Intel Arc | Dedicated AI acceleration chip |
| 🎮 GPU | 6GB NVIDIA | Primary model inference (fast!) |
| 💾 RAM | 32GB | Loads model weights |
| 🗄️ Storage | 1TB SSD | Stores model files |
| 🪟 OS | Windows 11 Home | — |

---

## 📦 Step 1 — Install Docker Desktop

Make sure you have **Docker Desktop** installed with the **AI features** enabled.

> Docker Desktop version must support the **Models** and **MCP Toolkit** sidebar items.

Download from: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

---

## 🤖 Step 2 — Understanding Model Selection

Before pulling a model, you need to understand three key concepts:

### 🔢 Parameters
> The "brain size" of an AI model. More parameters = smarter model, but also larger file size and more RAM needed.
```
7B  = 7 Billion parameters  → Good for most coding tasks ✅
30B = 30 Billion parameters → Needs 16GB+ VRAM
70B = 70 Billion parameters → Needs very high-end hardware
```

### 📦 Quantization
> Compression of the model. Like image compression — smaller file, slight quality trade-off.
```
F16  → Full precision, 16 bits/parameter → Largest, best quality
Q8_0 → 8 bits/parameter                 → Good balance
Q4_0 → 4 bits/parameter                 → ~4x smaller, great for most tasks ✅
Q2   → 2 bits/parameter                 → Smallest, noticeable quality loss
```

### 💡 RAM vs VRAM Loading Rule
```
Model fits in VRAM (6GB)?  → Loads on GPU  ⚡ Super fast!
Model too big for VRAM?    → Overflows to RAM 🐢 Slower
Model too big for RAM?     → ❌ Won't run at all
```

---

## 🏆 Step 3 — Pull the Right Model

### Our Pick: `qwen2.5:7B-Q4_0`

| Property | Value | Why It Matters |
|---|---|---|
| Parameters | 7.62B | Smart enough for coding tasks |
| Quantization | Q4_0 | 4x compressed, great quality |
| Size | 4.12 GiB | Fits perfectly in 6GB VRAM ✅ |
| Architecture | qwen2 | Optimized for code |

### How to Pull

1. Open **Docker Desktop**
2. Click **Models** in the left sidebar
3. Search for `qwen2.5`
4. Find `qwen2.5:7B-Q4_0` and click **Pull**

#### Sample Screenshot
<img width="1668" height="857" alt="image" src="https://github.com/user-attachments/assets/1e9118bf-4f88-4f58-8548-ba7565994153" />



> ⏳ This will download ~4GB. Make sure you have a good internet connection.

---

## ⚡ Step 4 — Enable GPU-Accelerated Inference

By default, Docker Model Runner uses CPU only. Enabling GPU support gives **10x faster** responses!

### Enable TCP + GPU Support

1. Open **Docker Desktop → Settings → AI**
2. Check ✅ **Enable Docker Model Runner**
3. Check ✅ **Enable host-side TCP support**
   - Port: `12434` (default — leave as is)
4. Check ✅ **Enable GPU-backed inference**
5. Click **Apply**

```
⚠️ Note: GPU-backed inference downloads additional components
   to ~/.docker/bin/inference — this may take a few minutes.
```

### Verify It's Working

Open your browser and navigate to:
```
http://localhost:12434/engines/v1/models
```

You should see a JSON response like:
```json
{
  "object": "list",
  "data": [
    {
      "id": "docker.io/ai/qwen2.5:7B-Q4_0",
      "object": "model",
      "created": 1745580818,
      "owned_by": "docker"
    }
  ]
}
```

✅ If you see this — your model is running and accessible via API!

### Verify GPU Usage

Go to **Docker Desktop → Models → Requests tab** after sending a prompt.

```
Duration: ~273ms  ← GPU is working! ⚡
Duration: ~3000ms ← CPU only 🐢
```
<img width="1662" height="515" alt="image" src="https://github.com/user-attachments/assets/091e9e33-7806-4612-ad52-9c906e708ff3" />

---

## 🔌 Step 5 — Connect VS Code via Continue.dev

Docker Models exposes a **local OpenAI-compatible REST API** at:
```
http://localhost:12434/engines/v1
```

This means ANY tool that supports OpenAI's API can use your local model — just change the URL!

### Install Continue.dev Extension

1. Open **VS Code**
2. Go to **Extensions** (`Ctrl + Shift + X`)
3. Search for `Continue`
4. Install **Continue - open-source AI code agent** (2.4M+ downloads)

### Configure Continue.dev

Open the config file at:
```
C:\Users\<yourname>\.continue\config.yaml
```

Replace everything with:

```yaml
name: Local Config
version: 1.0.0
schema: v1
models:
  - name: Qwen2.5 Coder Local
    provider: openai
    model: ai/qwen2.5:7B-Q4_0
    apiBase: http://localhost:12434/engines/v1
    apiKey: docker
```

> 💡 **Why `provider: openai`?** Docker's API is OpenAI-compatible — it speaks the same language, just at a different address (localhost instead of the cloud)!

> 💡 **Why `apiKey: docker`?** It's just a placeholder — no real authentication is needed for localhost APIs.

### Fix Windows PowerShell Execution Policy (if needed)

If you see script execution errors, run this in **PowerShell as Administrator**:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Reload and Test

1. Click **Reload** in the Continue panel (right sidebar in VS Code)
2. Select **Qwen2.5 Coder Local** from the model dropdown
3. Ask it something:

```
Can you explain what a Docker volume is?
```

✅ If it responds — **you're done!** You now have a free, private, GPU-accelerated AI coding assistant running 100% on your machine! 🎉

<img width="1280" height="1216" alt="image" src="https://github.com/user-attachments/assets/d169eb2e-f3a9-4b9e-b6a9-d577056561db" />

---

## 🏗️ How It All Works Together

```
┌─────────────────────────────────────────────┐
│              YOUR MACHINE                    │
│                                              │
│  ┌─────────────────┐                         │
│  │  Docker Models  │  ← qwen2.5:7B-Q4_0     │
│  │  (AI Brain) 🧠  │    runs on your GPU     │
│  └────────┬────────┘                         │
│           │ exposes                          │
│           ▼                                  │
│  ┌─────────────────┐                         │
│  │  localhost:12434│  ← OpenAI-compatible    │
│  │  REST API  🔌   │    just like -p 8080    │
│  └────────┬────────┘                         │
│           │ connects to                      │
│           ▼                                  │
│  ┌─────────────────┐                         │
│  │  VS Code        │  ← Continue.dev         │
│  │  (Your IDE) 💻  │    extension            │
│  └─────────────────┘                         │
└─────────────────────────────────────────────┘

No internet. No API costs. No data leaks.
```

---

## 🆚 Before vs After

| | Before (GitHub Copilot) | After (Local AI) |
|---|---|---|
| 🧠 AI Brain | GitHub's servers | Your GPU |
| 💸 Cost | $10-19/month | **FREE** |
| 🔒 Privacy | Code sent to GitHub | **Stays on your machine** |
| 🌐 Internet | Required | **Not needed** |
| ⚡ Speed | Depends on network | **~273ms on GPU** |
| 🛠️ Customizable | Limited | **Full control** |

---

## 🚀 Quick Reference

```bash
# Verify model API is running
curl http://localhost:12434/engines/v1/models

# Check Docker Model Runner status
# Docker Desktop → Models → Requests tab

# Config file location
# C:\Users\<yourname>\.continue\config.yaml
```

---

## ❓ Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| `Connection error` in Continue.dev | TCP not enabled | Docker Desktop → Settings → AI → Enable host-side TCP |
| Slow responses (>2s) | GPU not enabled | Docker Desktop → Settings → AI → Enable GPU-backed inference |
| `npx` script error | PowerShell policy | Run `Set-ExecutionPolicy RemoteSigned` as Admin |
| Model not showing | Not pulled yet | Docker Desktop → Models → Pull qwen2.5:7B-Q4_0 |

---

*📅 Guide created: March 2026 | 🐳 Docker Desktop with AI features (Beta)*
