# 🤖 AI Learning Roadmap — Avisek Dey
> *Tracking document for the AI Engineer journey. Updated after every session.*
> Last updated: 2026-03-29

---

## 👤 Profile Snapshot

| Attribute | Detail |
|---|---|
| Name | Avisek Dey |
| Role | Senior Analyst @ Ernst & Young, Kolkata |
| Experience | 7+ years Python, Django, Docker, Azure, REST APIs |
| Learning Style | Self-taught, hands-on, project-first |
| Goal | Build AI-powered products + Transition to AI/ML Engineer |
| Time Available | 3–5 hrs/week |
| Showcase | GitHub (Knowledge Vault) + TIL log |

---

## 🧠 Starting Knowledge Baseline (Assessed: 2026-03-29)

| Area | Level | Notes |
|---|---|---|
| Python | ⭐⭐⭐⭐⭐ | Expert. 7+ years production use |
| Django / REST APIs / Docker | ⭐⭐⭐⭐ | Production-grade |
| ML Fundamentals | 🔴 Pre-beginner | Knows terms, zero hands-on |
| Deep Learning / NNs | 🔴 Beginner | Buzzword level |
| LLMs / GenAI | 🔴 Beginner | Casual ChatGPT user |
| Computer Vision | 🟡 Some exposure | EY project (document categorization) |
| Math (Linear Algebra, Stats, Calculus) | 🟡 School level | Knows y=mx+c, basic calculus |

---

## 🗺️ The Roadmap — 3 Phases, ~6–7 Months

### Reference: [roadmap.sh/ai-engineer](https://roadmap.sh/ai-engineer)


### Repository: [ai-engineer-journey](https://github.com/Riju007/ai-engineer-journey)

---

## 🔵 PHASE 1 — ML Foundations
**Target Duration:** 6–8 weeks | **Status:** 🔜 Not Started

### 📚 Topics to Cover
- [ ] What is ML? Supervised / Unsupervised / Reinforcement Learning
- [ ] Linear Regression (the math behind y = mx + c)
- [ ] Logistic Regression + Classification
- [ ] Decision Trees & Random Forests
- [ ] Overfitting, Bias-Variance tradeoff
- [ ] Train/Test/Validation split & Cross-validation
- [ ] Evaluation metrics (Accuracy, Precision, Recall, F1, RMSE)
- [ ] NumPy & Pandas deep dive
- [ ] Intro to scikit-learn

### 🛠️ Projects

| # | Project | Description | Status | GitHub Link |
|---|---|---|---|---|
| P1 | 🏠 House Price Predictor | Linear Regression — scikit-learn | 🔜 | — |
| P2 | 📧 Spam Email Classifier | Logistic Regression + text features | 🔜 | — |
| P3 | 🚢 Titanic Survival Predictor | Decision Trees + Random Forest (Kaggle) | 🔜 | — |
| P4 | 📊 EY-flavored ML Pipeline | End-to-end: raw data → prediction API (FastAPI) | 🔜 | — |

### 📝 TIL Entries from Phase 1
*(To be filled as you learn)*

---

## 🟣 PHASE 2 — Deep Learning & Neural Networks
**Target Duration:** 8–10 weeks | **Status:** 🔒 Locked (starts after Phase 1)

### 📚 Topics to Cover
- [ ] How a Neural Network works (perceptron → layers → backprop)
- [ ] Activation functions (ReLU, Sigmoid, Softmax)
- [ ] Loss functions & Gradient Descent (intuition-first)
- [ ] PyTorch basics
- [ ] Convolutional Neural Networks (CNNs)
- [ ] Transfer Learning (using pre-trained models)
- [ ] Transformers & Attention mechanism (intuition)
- [ ] HuggingFace Pipelines

### 🛠️ Projects

| # | Project | Description | Status | GitHub Link |
|---|---|---|---|---|
| P5 | 🧠 NN from Scratch | Pure Python neural network — no frameworks | 🔒 | — |
| P6 | 🖼️ Image Classifier (CNN) | PyTorch — MNIST → custom images | 🔒 | — |
| P7 | 💬 Sentiment Analyzer | HuggingFace — fine-tune for text classification | 🔒 | — |
| P8 | 📄 Document Type Classifier | Upgrade of EY CV tool — CNN or Transformer | 🔒 | — |

### 📝 TIL Entries from Phase 2
*(To be filled as you learn)*

---

## 🟡 PHASE 3 — GenAI Engineering *(The Power Zone)*
**Target Duration:** 8–10 weeks | **Status:** 🔒 Locked (starts after Phase 2)

### 📚 Topics to Cover
- [ ] Prompt Engineering patterns (Chain-of-Thought, Few-shot, ReAct)
- [ ] OpenAI / Anthropic API mastery
- [ ] RAG — Retrieval Augmented Generation
- [ ] Vector Databases (FAISS, ChromaDB)
- [ ] LangChain / LlamaIndex
- [ ] AI Agents & Tool use
- [ ] Evaluation of LLM outputs
- [ ] Deployment of AI apps (FastAPI + Docker + Azure)

### 🛠️ Projects

| # | Project | Description | Status | GitHub Link |
|---|---|---|---|---|
| P9 | ⚙️ Prompt Engineering Toolkit | Personal library of reusable prompt patterns | 🔒 | — |
| P10 | 🔌 AI-Powered REST API | FastAPI + OpenAI/Anthropic API | 🔒 | — |
| P11 | 📚 RAG Document Q&A Bot | LangChain + FAISS + PDF loader (EY use case!) | 🔒 | — |
| P12 | 🤖 AI Agent with Tools | Autonomous agent — search, read files, call APIs | 🔒 | — |
| P13 | 🏆 Flagship AI Product | Your capstone — design it yourself | 🔒 | — |

### 📝 TIL Entries from Phase 3
*(To be filled as you learn)*

---

## 📊 Overall Progress Tracker

| Phase | Projects Total | Projects Done | Topics Done | Status |
|---|---|---|---|---|
| Phase 1 — ML Foundations | 4 | 0/4 | 0/9 | 🔜 Not Started |
| Phase 2 — Deep Learning | 4 | 0/4 | 0/8 | 🔒 Locked |
| Phase 3 — GenAI Engineering | 5 | 0/5 | 0/8 | 🔒 Locked |
| **Total** | **13** | **0/13** | **0/25** | **0%** |

---

## 🐙 GitHub Structure (Knowledge Vault)

```
knowledge-vault/
│
├── README.md
│
├── docker-notes/
│   └── local-ai-setup-guide.md
│
├── til/
│   └── 2026-03-28-docker-models-tcp.md
│
├── ai-learning/
    ├── AI_LEARNING_ROADMAP.md
│   ├── phase-1-ml-foundations/
│   │   ├── notes/
│   │   └── projects/
│   │       ├── P1-house-price-predictor/
│   │       ├── P2-spam-classifier/
│   │       ├── P3-titanic-survival/
│   │       └── P4-ey-ml-pipeline/
│   │
│   ├── phase-2-deep-learning/
│   │   ├── notes/
│   │   └── projects/
│   │       ├── P5-nn-from-scratch/
│   │       ├── P6-image-classifier-cnn/
│   │       ├── P7-sentiment-analyzer/
│   │       └── P8-document-classifier/
│   │
│   └── phase-3-genai-engineering/
│       ├── notes/
│       └── projects/
│           ├── P9-prompt-toolkit/
│           ├── P10-ai-rest-api/
│           ├── P11-rag-doc-qa-bot/
│           ├── P12-ai-agent/
│           └── P13-flagship-product/
│
└── system-design/                   ← Coming later
```

---

## 📝 Session Log

| Date | Session # | What We Covered | Next Up |
|---|---|---|---|
| 2026-03-29 | #1 | Assessed baseline. Built roadmap. Planned all 13 projects. Decided on Knowledge Vault repo structure. | Start Phase 1 — Week 1 |

---

## 🔖 Key Resources

| Resource | Link | Phase |
|---|---|---|
| roadmap.sh AI Engineer | https://roadmap.sh/ai-engineer | All |
| Kaggle (datasets + competitions) | https://www.kaggle.com | Phase 1 |
| scikit-learn docs | https://scikit-learn.org | Phase 1 |
| PyTorch tutorials | https://pytorch.org/tutorials | Phase 2 |
| HuggingFace | https://huggingface.co | Phase 2–3 |
| LangChain docs | https://docs.langchain.com | Phase 3 |
| FastAPI docs | https://fastapi.tiangolo.com | Phase 1 + 3 |

---

*🔄 This document is updated after every learning session.*
*📌 Rule: Never mark a project done without a GitHub link.*
