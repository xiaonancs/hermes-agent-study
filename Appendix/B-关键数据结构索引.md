# 附录 B — 关键数据结构索引

> 核心类、函数、数据结构的速查表，方便读者在阅读源码时快速定位。

---

## 核心类

| 类名 | 文件 | 行数 | 职责 |
|------|------|------|------|
| `AIAgent` | `run_agent.py` | ~11,487 | 核心对话循环、工具调用、压缩、持久化 |
| `HermesCLI` | `cli.py` | ~10,033 | 交互式终端 UI、输入处理、命令分发 |
| `GatewayRunner` | `gateway/run.py` | ~9,798 | 消息网关、平台路由、会话管理 |
| `SessionDB` | `hermes_state.py` | ~1,239 | SQLite 会话存储、FTS5 搜索 |
| `ToolEntry` | `tools/registry.py` | ~80 | 工具元数据（schema, handler, check_fn） |
| `MemoryManager` | `agent/memory_manager.py` | ~374 | 记忆编排（内置 + 外部 Provider） |
| `MemoryProvider` | `agent/memory_provider.py` | ~60 | 记忆 Provider 抽象基类 |
| `ContextEngine` | `agent/context_engine.py` | ~50 | 上下文引擎抽象基类 |

## 核心函数

| 函数 | 文件 | 职责 |
|------|------|------|
| `run_conversation()` | `run_agent.py` | 主对话循环 |
| `build_system_prompt()` | `agent/prompt_builder.py` | 组装系统提示词 |
| `resolve_runtime_provider()` | `hermes_cli/runtime_provider.py` | 解析 Provider 到 api_mode/key/url |
| `handle_function_call()` | `model_tools.py` | 分发工具调用 |
| `get_tool_definitions()` | `model_tools.py` | 收集工具 schema |
| `discover_builtin_tools()` | `tools/registry.py` | AST 扫描发现自注册工具 |
| `registry.register()` | `tools/registry.py` | 工具注册 |
| `_scan_context_content()` | `agent/prompt_builder.py` | Prompt 注入防御扫描 |
| `sanitize_context()` | `agent/memory_manager.py` | 记忆上下文清洗 |
| `build_memory_context_block()` | `agent/memory_manager.py` | 构建记忆注入块 |
| `load_hermes_dotenv()` | `hermes_cli/env_loader.py` | .env 文件加载 |

## 关键数据结构

### SQLite Schema（hermes_state.py, SCHEMA_VERSION 6）

```sql
sessions (
    id TEXT PRIMARY KEY,
    source TEXT NOT NULL,           -- cli, telegram, discord...
    user_id TEXT,
    model TEXT,
    model_config TEXT,
    system_prompt TEXT,
    parent_session_id TEXT,         -- compression-triggered splitting
    started_at REAL NOT NULL,
    ended_at REAL,
    end_reason TEXT,
    message_count INTEGER DEFAULT 0,
    tool_call_count INTEGER DEFAULT 0,
    input_tokens INTEGER DEFAULT 0,
    output_tokens INTEGER DEFAULT 0,
    cache_read_tokens INTEGER DEFAULT 0,
    cache_write_tokens INTEGER DEFAULT 0,
    reasoning_tokens INTEGER DEFAULT 0,
    billing_provider TEXT,
    billing_base_url TEXT
)
```

### SKILL.md 格式

```yaml
---
name: skill-name
description: One-line description
When to Use: Trigger conditions
---

# Skill Name

## Procedure
1. Step 1
2. Step 2

## Pitfalls
- Common mistake 1

## Verification
- Check 1
```

### 配置文件结构（~/.hermes/config.yaml）

```yaml
model: provider:model-name
provider: openrouter
api_key: sk-...
tools:
  enabled: [terminal, file, web, browser]
  disabled: []
skills:
  enabled: [all]
memory:
  provider: builtin
gateway:
  platforms: [telegram, discord]
```

## 文件路径速查

| 类别 | 路径 |
|------|------|
| 用户配置 | `~/.hermes/config.yaml` |
| API 密钥 | `~/.hermes/.env` |
| 记忆文件 | `~/.hermes/memory/MEMORY.md` |
| 用户画像 | `~/.hermes/memory/USER.md` |
| 人格文件 | `~/.hermes/SOUL.md` |
| 技能目录 | `~/.hermes/skills/` |
| 会话数据库 | `~/.hermes/state.db` |
| 网关 PID | `~/.hermes/gateway.pid` |
| 定时任务 | `~/.hermes/cron/jobs.json` |
| 插件目录 | `~/.hermes/plugins/` |
