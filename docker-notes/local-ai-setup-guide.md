# 🐳 Running AI Locally with Docker Desktop
### A step-by-step guide to replacing GitHub Copilot with a free, private, GPU-accelerated local AI

> 📝 Also published on [dev.to](https://dev.to/riju005/i-tried-replacing-github-copilot-with-local-ai-heres-what-happened-docker-gpu-5b44)

---

## ⚠️ Reality Check First

This is NOT a perfect replacement for GitHub Copilot.

Cloud models (GPT-4/5-level) are still better at reasoning, large codebases, and consistency.

But for **most day-to-day coding tasks**, a local setup is fast enough, smart enough, and WAY more private. Think of this as a **practical alternative**, not a 1:1 replacement.

### ❌ When You Should NOT Use This
- Working on large enterprise codebases
- Need best-in-class reasoning (GPT-4 level)
- Want zero setup / plug-and-play
- Low-end hardware (<16GB RAM, no GPU)

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

> This guide is based on my machine. Adjust model size based on your hardware.

| Component | Spec | Role in AI Inference |
|---|---|---|
| 💻 Laptop | Lenovo IdeaPad Pro 5 16IMH9 | — |
| 🧠 CPU | Intel Core Ultra 9 185H | Handles overflow inference |
| ⚡ NPU | Intel Arc | Dedicated AI acceleration chip |
| 🎮 GPU | 6GB NVIDIA | Primary model inference (fast!) |
| 💾 RAM | 32GB | Loads model weights |
| 🗄️ Storage | 1TB SSD | Stores model files |
| 🪟 OS | Windows 11 Home | — |

### Minimum Setup Recommendation

| Hardware | What to Expect |
|---|---|
| No GPU | Works, but slow (~2–5s responses) |
| 8GB RAM | Very limited models only |
| 16GB RAM | Usable |
| 32GB + GPU | 🔥 Ideal |

---

## 📦 Step 1 — Install Docker Desktop

Make sure you have **Docker Desktop** installed with the **AI features** enabled.

> Docker Desktop version must support the **Models** and **MCP Toolkit** sidebar items.

Download from: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

---

## 🤖 Step 2 — Understanding Model Selection

Before pulling a model, you need to understand three key concepts:

### 🔢 Parameters = Brain Size
> More parameters = smarter model, but larger file size and more RAM needed.
```
7B  = 7 Billion parameters  → Good for most coding tasks ✅
30B = 30 Billion parameters → Needs 16GB+ VRAM
70B = 70 Billion parameters → Needs very high-end hardware
```

### 📦 Quantization = Compression
> Like image compression — smaller file, slight quality trade-off.
```
F16  → Full precision, 16 bits/parameter → Largest, best quality
Q8_0 → 8 bits/parameter                 → Good balance
Q4_0 → 4 bits/parameter                 → ~4x smaller, great for most tasks ✅
Q2   → 2 bits/parameter                 → Smallest, noticeable quality loss
```

### 💡 The Golden Rule — RAM vs VRAM
```
Model fits in VRAM (6GB)?  → Loads on GPU  ⚡ Super fast!
Model too big for VRAM?    → Overflows to RAM 🐢 Slower
Model too big for RAM?     → ❌ Won't run at all
```

---

## 🏆 Step 3 — Pull the Right Model

### Recommended: `qwen2.5:7B-Q4_0`

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

<img width="1668" height="857" alt="Docker Models search showing qwen2.5 variants" src="https://github.com/user-attachments/assets/1e9118bf-4f88-4f58-8548-ba7565994153" />

> ⏳ This will download ~4GB. Make sure you have a good internet connection.

---

## ⚡ Step 4 — Enable GPU-Accelerated Inference

**Most guides miss this step entirely.**

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

### Verify GPU Usage

Go to **Docker Desktop → Models → Requests tab** after sending a prompt.

<img width="1662" height="515" alt="Docker Models Requests tab showing 273ms response time" src="https://github.com/user-attachments/assets/091e9e33-7806-4612-ad52-9c906e708ff3" />

| Mode | Speed |
|---|---|
| GPU ⚡ | ~273ms |
| CPU 🐢 | ~3000ms |

**That's a 10x speedup from one checkbox.**

---

## 🔌 Step 5 — Connect VS Code via Continue.dev

Docker Models exposes a **local OpenAI-compatible REST API** at:
```
http://localhost:12434/engines/v1
```

> Key insight: **any tool that supports OpenAI's API works here** — just change the URL from OpenAI's server to localhost.

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

> 💡 **Why `provider: openai`?** Docker's API is OpenAI-compatible — same language, different address.

> 💡 **Why `apiKey: docker`?** Just a placeholder — no real authentication needed for localhost.

### Windows PowerShell Fix (if needed)

If you see script execution errors, run this in **PowerShell as Administrator**:

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

### Reload and Test

1. Click **Reload** in the Continue panel
2. Select **Qwen2.5 Coder Local** from the model dropdown
3. Ask it something:

```
Can you explain what a Docker volume is?
```

<img width="1280" height="1216" alt="Continue.dev working with local Qwen model in VS Code" src="https://github.com/user-attachments/assets/d169eb2e-f3a9-4b9e-b6a9-d577056561db" />

✅ If it responds — **you're done!**

---

## 🧪 Real Comparison — Copilot vs Local AI

### Prompt tested:
```
Write a Django REST Framework viewset for a User model
with JWT authentication and permission classes
```

### GitHub Copilot:
Clean, complete, production-ready. ~60 lines, zero follow-up needed.

### Local Qwen 7B:
```python
from rest_framework import viewsets, permissions
from rest_framework_simplejwt.authentication import JWTAuthentication
from .models import User
from .serializers import UserSerializer

class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    authentication_classes = [JWTAuthentication]
    permission_classes = [permissions.IsAuthenticated]

    def get_queryset(self):
        # Users can only see their own data
        return User.objects.filter(id=self.request.user.id)
```

Solid and functional — but needed a follow-up prompt for edge cases.

### Verdict:

| Feature | Copilot | Local Qwen 7B |
|---|---|---|
| Speed | Fast | Fast (GPU ⚡) |
| Boilerplate | Excellent | Good |
| Reasoning | Strong | Moderate |
| Multi-file context | Better | Limited |
| Cost | $10–19/mo | **FREE** |
| Privacy | External servers | **Your machine** |

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

## ⚠️ Where This Falls Short

- ❌ Not as smart as GPT-4-level models
- ❌ Limited context window (struggles with large codebases)
- ❌ Needs decent hardware for best results
- ❌ Setup takes 30–60 minutes vs just paying for Copilot

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

## 🚀 What's Next?

This is just the foundation. Docker's **MCP Toolkit** can let your local AI actually *act* — read your codebase, modify files, understand requirements. That's a full agent setup — coming in Part 2.

---

*📅 Guide created: March 2026 | 🐳 Docker Desktop with AI features*