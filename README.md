# Personal Chief (私厨) 🍳

AI 驱动的私人厨师助手 —— 上传食材照片或文字描述，即可获得智能食谱推荐。

## 功能

- **多模态识别** — 识别用户上传的食材照片，评估新鲜度与可用量
- **智能搜索** — 基于可用食材，通过 Tavily 搜索匹配菜谱
- **评分排序** — 从营养价值与制作难度两个维度量化评分，推荐最优食谱
- **流式对话** — SSE 实时流式响应，支持多轮对话与历史记录
- **图片上传** — 集成阿里云 OSS 预签名 URL，前端直传

## 技术栈

| 层 | 技术 |
|---|---|
| 后端 | Python 3.13+, FastAPI |
| AI 框架 | LangChain, LangGraph, LangGraph Checkpoint |
| 大模型 | Qwen 3.5 Omni Flash (DashScope) |
| 搜索 | Tavily Search API |
| 存储 | SQLite (会话持久化), 阿里云 OSS (图片) |
| 前端 | Next.js (预构建静态导出) |
| 项目工具 | uv |

## 快速开始

### 前置要求

- Python >= 3.13
- [uv](https://docs.astral.sh/uv/) 包管理器

### 安装

```bash
# 克隆项目
git clone <repo-url>
cd Ic-course

# 创建虚拟环境并安装依赖
uv venv
uv sync
```

### 配置环境变量

复制 `.env` 文件并填写以下密钥：

| 变量 | 说明 |
|---|---|
| `DASHSCOPE_API_KEY` | 阿里云 DashScope API 密钥 |
| `TAVILY_API_KEY` | Tavily 搜索 API 密钥 |
| `LANGSMITH_API_KEY` | LangSmith 追踪密钥（可选） |
| `OSS_ACCESS_KEY_ID` | 阿里云 OSS AccessKey |
| `OSS_ACCESS_KEY_SECRET` | 阿里云 OSS AccessKey Secret |
| `OSS_BUCKET` | OSS 存储桶名称 |

### 启动

```bash
# 启动服务（监听 127.0.0.1:8001）
python -m app.main
```

浏览器打开 `http://127.0.0.1:8001` 即可使用。

## API 接口

| 方法 | 路径 | 说明 |
|---|---|---|
| POST | `/api/v1/chat/stream` | 流式对话（SSE） |
| GET | `/api/v1/chat/messages?thread_id=...` | 获取会话历史 |
| DELETE | `/api/v1/chat/messages?thread_id=...` | 清空会话 |
| GET | `/api/v1/oss/presign?filename=...` | 获取 OSS 上传签名 |

完整 API 文档请访问 `/docs`（Swagger UI）。

## 项目结构

```
├── app/
│   ├── main.py                 # FastAPI 入口，路由与静态文件
│   ├── agents/
│   │   └── personal_chief.py   # LangGraph Agent 核心逻辑
│   ├── api/v1/
│   │   ├── chat.py             # 对话接口
│   │   └── oss.py              # OSS 签名接口
│   ├── common/logger.py        # 日志配置
│   ├── model/schemas.py        # Pydantic 数据模型
│   └── static/                 # 前端静态文件
├── db/personal_chief.db        # SQLite 数据库
├── pyproject.toml              # 项目配置与依赖
└── langgraph.json              # LangGraph 配置
```
