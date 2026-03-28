# 💡 TIL — Docker Model Runner Needs TCP Explicitly Enabled

**Date:** 2026-03-28
**Category:** Docker, Local AI, DevOps
**Time to figure out:** ~30 mins of debugging 😅

---

## 🔍 What I Was Trying to Do

Connect **Continue.dev** (VS Code AI extension) to a locally running AI model via **Docker Desktop's Models feature**.

The model was running fine — I could chat with it directly inside Docker Desktop. But Continue.dev kept throwing:

```
Connection error.
Error handling model response from Qwen2.5 Coder Local.
```

---

## 🧠 What I Assumed

I assumed that since the model was running in Docker Desktop, it would automatically be accessible at `localhost:12434` — just like a Docker container with `-p 12434:12434`.

**Wrong.** 😄

---

## 💡 What I Actually Learned

Docker Model Runner exposes the API in **two different ways**:

```
1. Via Docker socket  → /var/run/docker.sock
   (only works for other Docker containers)

2. Via TCP localhost  → http://localhost:12434
   (works for HOST applications like VS Code)
```

By default, **only the socket is enabled**. Host applications like Continue.dev need TCP — which must be turned on manually.

---

## ✅ The Fix

**Docker Desktop → Settings → AI → Enable host-side TCP support ✅**

Or via CLI:
```bash
docker desktop enable model-runner --tcp=12434
```

Then verify it's working:
```bash
curl http://localhost:12434/engines/v1/models
```

---

## 🔗 The Correct API Endpoint

```
http://localhost:12434/engines/v1
```

> Note: Older docs mention `/engines/llama.cpp/v1` — this has been simplified to just `/engines/v1` in recent Docker Desktop versions.

---

## 🎯 Bonus Learning — GPU Inference Is Not Default Either

Even after TCP was enabled, I discovered GPU acceleration also needs to be explicitly turned on:

**Docker Desktop → Settings → AI → Enable GPU-backed inference ✅**

Before GPU: `~3000ms` per response 🐢
After GPU: `~273ms` per response ⚡

**That's a ~10x speedup from one checkbox!**

---

## 📦 My Setup

```
Laptop : Lenovo IdeaPad Pro 5
GPU    : 6GB NVIDIA
Model  : qwen2.5:7B-Q4_0 (4.12 GiB)
Tool   : Continue.dev v1.2.19 in VS Code
```

---

## 🔗 Related

- [Full Guide: Running AI Locally with Docker](../docker-notes/local-ai-setup-guide.md)
- [Docker Model Runner Docs](https://docs.docker.com/ai/model-runner/)

---

*Short and sharp — exactly how debugging should be documented* 🎯
