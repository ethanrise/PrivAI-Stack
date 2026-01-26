# PrivAI-Stack


**PrivAI-Stack**  (Private AI Stack) is an all-in-one AI scaffolding solution designed specifically for private deployment. It integrates four of the most powerful open-source components available today, aiming to help developers and enterprises quickly set up a local AI workflow with **enterprise-grade document parsing capabilities** and **ultra-fast inference speed**.

> 🇨🇳 [中文版 README](./README_ch.md)
> 
## 🚀 Core Components Overview

The project consists of four independent modules, each isolated via Docker:

*   **`docling/`** (Open-sourced by IBM): Converts complex PDFs and documents accurately into Markdown/JSON—ideal for RAG preprocessing.
*   **`vllm/`**: Ultra-fast inference backend supporting PagedAttention, suitable for deploying mainstream large models such as DeepSeek and Qwen.
*   **`xinference/`**: A large model management platform that wraps vLLM at a higher level, providing unified management for LLMs, Embedding, and Rerank models.
*   **`dify/`**: An open-source LLMOps platform responsible for orchestrating workflows, managing knowledge bases, and publishing AI applications.

---

## 📂 Project Directory Structure

```text
PrivAI-Stack/
├── docling/         # Docling API service (parsing layer)
├── vllm/            # vLLM inference engine (compute layer)
├── xinference/      # Rerank and embedding model inference engine (compute layer)
└── dify/            # AI application orchestration platform (application layer)
```

每个文件夹中均包含：
- `.env`: Environment variable configuration (model paths, ports, API keys, etc.).
- `docker-compose.yml`: Container orchestration definition.

---

## 🛠️  Quick Start

### 1. Clone the repository
```bash
git clone https://github.com/your-username/PrivAI-Stack.git
cd PrivAI-Stack
```

### 2. Start services one by one

The project uses a decoupled design—start only what you need.

#### A. Deploy the inference layer (vLLM & Xinference)
It's recommended to start inference services first, as they are prerequisites for upper-layer applications.
```bash
cd xinference
# Modify model storage path in .env if needed
docker compose up -d
# View logs
docker compose logs -f
```

#### B. Deploy document parser (Docling)
```bash
cd ../docling
docker compose up -d
docker compose logs -f
```

#### C. Deploy application layer (Dify)
```bash
cd ../dify
docker compose up -d
docker compose logs -f
```

---

## 📖 Operational Guide

### Common Commands
Each module follows the same operational pattern:

| Action | Command |
| :--- | :--- |
| **Start service** | `docker compose up -d` |
| **Stop service** | `docker compose down` |
| **View live logs** | `docker compose logs -f` |
| **Restart service** | `docker compose restart` |

### Inter-module Communication
1.  **Connect Dify to Xinference**: 
    In Dify Admin → Settings → Model Providers → Choose Xinference, and enter `http://xinference:9997`.
2.  **Connect Dify to vLLM**: 
    In Dify Admin → Settings → Model Providers → Choose vLLM, and enter `http://vllm:8000/v1`.
3.  **Use Docling in Dify**: 
    In Dify Workflow, add an HTTP node and call Docling’s API at `http://docling:5001/v1/convert/file`

---

## ⚠️ Notes

1. **Hardware Requirement**: At least 24GB GPU VRAM (e.g., RTX 3090/4090) is recommended for smooth performance.
2. **Driver Setup**: Ensure the host has the `NVIDIA Container Toolkit` installed.
3. **Network Configuration**: By default, modules communicate via the host IP. Make sure ports defined in `.env` are not occupied.


---

## 🤝 Contributions & Feedback

If you encounter bugs or have suggestions for improvement, feel free to open an Issue!

*   **Author**: Your Name
*   **Star**: If this project helps you, please give it a ⭐!

---

### 💡 Why Choose PrivAI-Stack?

*   **Fully Decoupled**: Use only `docling` as a parsing tool, or just `vllm` as an inference API—no forced integration.
*   **Simple & Clean**: No complex install scripts—only basic `.env` and `docker-compose.yml`.
*   **Production-Ready**: All components are industry-leading tools for private AI deployments.

