# PrivAI-Stack


**PrivAI-Stack** (private ai stack) 是一套专为私有化部署打造的 AI 全栈脚手架。它集成了目前开源界最强的四个组件，旨在帮助开发者和企业一键搭建具备“企业级文档解析能力”和“极致推理速度”的本地 AI 工作流。

## 🚀 核心组件介绍

项目由四个独立的模块组成，每个模块均通过 Docker 进行隔离：

*   **`docling/`** (IBM 开源): 负责将复杂的 PDF、文档精准转换为 Markdown/JSON，是 RAG 预处理的神器。
*   **`vllm/`** : 极速推理后端，支持 PagedAttention，适合部署 DeepSeek、Qwen 等主流大模型。
*   **`xinference/`**: 大模型管理平台，作为 vLLM 的上层封装，统一管理 LLM、Embedding 和 Rerank 模型。
*   **`dify/`**: 开源的 LLMOps 平台，负责编排工作流、管理知识库及发布 AI 应用。

---

## 📂 项目目录结构

```text
PrivAI-Stack/
├── docling/         # Docling API 服务 (解析层)
├── vllm/            # vLLM 推理引擎 (算力层)
├── xinference/      # rerank和embedding模型推理引擎（算力层）
└── dify/            # AI 应用编排平台 (应用层)
```

每个文件夹中均包含：
- `.env`: 环境变量配置（模型路径、端口、API Key等）。
- `docker-compose.yml`: 容器编排定义。

---

## 🛠️ 快速上手

### 1. 克隆项目
```bash
git clone https://github.com/your-username/PrivAI-Stack.git
cd PrivAI-Stack
```

### 2. 逐一启动服务

项目采用独立部署设计，请按需进入目录启动：

#### A. 部署推理层 (vLLM & Xinference)
建议先启动推理服务，因为它是后续应用的基础。
```bash
cd xinference
# 根据需要修改 .env 中的模型存储路径
docker compose up -d
# 查看日志
docker compose logs -f
```

#### B. 部署文档解析 (Docling)
```bash
cd ../docling
docker compose up -d
docker compose logs -f
```

#### C. 部署应用层 (Dify)
```bash
cd ../dify
docker compose up -d
docker compose logs -f
```

---

## 📖 操作指南

### 常用命令
每个模块都遵循统一的操作规范：

| 操作 | 命令 |
| :--- | :--- |
| **启动服务** | `docker compose up -d` |
| **停止服务** | `docker compose down` |
| **查看实时日志** | `docker compose logs -f` |
| **重启服务** | `docker compose restart` |

### 模块间互联
1.  **Dify 连接 Xinference**: 
    在 Dify 管理后台 -> 设置 -> 模型供应商 -> 选择 Xinference，填写 `http://xinference:9997`。
2.  **Dify 连接  vLLM**: 
    在 Dify 管理后台 -> 设置 -> 模型供应商 -> 选择 vLLM，填写 `http://vllm:8000/v1`。
3.  **Dify 使用 Docling**: 
    在 Dify Workflow 中添加 HTTP 节点，调用 Docling 服务的 API，地址填写`http://docling:5001/v1/convert/file`

---

## ⚠️ 注意事项

1.  **硬件要求**: 建议至少配备 24G 显存（如 RTX 3090/4090）以获得流畅体验。
2.  **驱动程序**: 确保宿主机已安装 `NVIDIA Container Toolkit`。
3.  **网络配置**: 默认各模块通过宿主机 IP 进行通信，请确保 `.env` 中的端口未被占用。

---

## 🤝 贡献与反馈

如果你在使用过程中发现了 Bug 或有更好的优化建议，欢迎提交 Issue

*   **Author**: Your Name
*   **Star**: 如果这个项目对你有帮助，请给个 Star ⭐！

---

### 💡 为什么选择 PrivAI-Stack？
*   **完全解耦**: 你可以只启动 `docling` 用作工具，或者只启动 `vllm` 对外提供接口。
*   **简单纯粹**: 没有复杂的安装脚本，只有最基础的 `.env` 和 `docker-compose`。
*   **生产可用**: 选用的都是目前业内公认最强的私有化组件。

