# Hermes Agent 深度研究

**从机制本质到代码实现，从 OpenClaw 渊源到 EvoMap 争议**

---

## 这本书在说什么

Hermes Agent 是 2026 年最火的开源 AI Agent 项目——90k Stars，Nous Research 出品，口号是"The agent that grows with you"。

但它的火爆背后有两个绕不开的问题：

1. 整体架构（Gateway、Tool Registry、Skill System、启动文件体系）跟 OpenClaw（358k Stars）高度一致，只是从 TypeScript 换成了 Python，还内置了 `hermes claw migrate` 迁移工具。
2. 核心差异化卖点"自进化"被中国团队 EvoMap 指控架构级抄袭——10 步主循环对齐、12 组术语替换、三层记忆对应、7 份材料零引用。

本书从源码层面拆解 Hermes Agent，同时代码级对比三个项目，用事实说话。

---

## 全书结构

### 总纲

| 文档 | 内容 |
|------|------|
| [总纲 — Hermes Agent 技术主线分析](总纲-Hermes-Agent技术主线分析.md) | 核心机制、火爆原因、设计哲学、架构全貌 |
| [全网调研 — 社区认知地图](全网调研-社区认知地图.md) | 中英文社区技术分析索引、观点争议、认知盲区 |

### Part I 原理与使用

Hermes Agent 是什么、怎么工作、核心概念。读完这部分你能完整理解它的设计逻辑。

| 章节 | 主题 | 核心源码 |
|------|------|---------|
| [01](Part%20I%20原理与使用/01-项目全景与设计哲学.md) | 项目全景与设计哲学 | `run_agent.py`, `AGENTS.md` |
| [02](Part%20I%20原理与使用/02-启动流程与配置系统.md) | 启动流程与配置系统 | `hermes_cli/main.py`, `config.py` |
| [03](Part%20I%20原理与使用/03-Agent核心循环.md) | Agent 核心循环 | `run_agent.py` (11,487 行) |
| [04](Part%20I%20原理与使用/04-SystemPrompt组装.md) | System Prompt 组装 | `agent/prompt_builder.py` |
| [05](Part%20I%20原理与使用/05-Provider与API模式.md) | Provider 与 API 模式 | `runtime_provider.py`, `auth.py` |
| [06](Part%20I%20原理与使用/06-记忆系统.md) | 记忆系统 | `memory_manager.py`, `memory_tool.py` |
| [07](Part%20I%20原理与使用/07-Skill系统.md) | Skill 系统 | `skill_commands.py`, `skills_hub.py` |
| [08](Part%20I%20原理与使用/08-自进化引擎.md) | 自进化引擎 | `hermes-agent-self-evolution/` |
| [09](Part%20I%20原理与使用/09-状态管理与Profile隔离.md) | 状态管理与 Profile 隔离 | `hermes_constants.py` |

### Part II 源码分析

逐模块拆解实现细节。每章追踪关键代码路径，分析设计权衡。

| 章节 | 主题 | 核心源码 |
|------|------|---------|
| [10](Part%20II%20源码分析/10-上下文压缩与缓存.md) | 上下文压缩与缓存 | `context_compressor.py`, `prompt_caching.py` |
| [11](Part%20II%20源码分析/11-会话持久化.md) | 会话持久化 | `hermes_state.py`, `session.py` |
| [12](Part%20II%20源码分析/12-工具系统总览.md) | 工具系统总览 | `tools/registry.py`, `toolsets.py` |
| [13](Part%20II%20源码分析/13-终端与沙箱执行.md) | 终端与沙箱执行 | `terminal_tool.py`, `environments/` |
| [14](Part%20II%20源码分析/14-文件与代码工具.md) | 文件与代码工具 | `file_tools.py`, `code_execution_tool.py` |
| [15](Part%20II%20源码分析/15-浏览器自动化.md) | 浏览器自动化 | `browser_tool.py` |
| [16](Part%20II%20源码分析/16-MCP集成.md) | MCP 集成 | `mcp_tool.py` |
| [17](Part%20II%20源码分析/17-子Agent委托.md) | 子 Agent 委托 | `delegate_tool.py` |
| [18](Part%20II%20源码分析/18-定时任务.md) | 定时任务 | `cron/` |
| [19](Part%20II%20源码分析/19-Gateway网关.md) | Gateway 网关 | `gateway/run.py` (9,798 行) |
| [20](Part%20II%20源码分析/20-插件系统.md) | 插件系统 | `plugins.py`, `plugins/` |
| [21](Part%20II%20源码分析/21-ACP-IDE集成.md) | ACP IDE 集成 | `acp_adapter/` |
| [22](Part%20II%20源码分析/22-安全与权限.md) | 安全与权限 | `approval.py`, `skills_guard.py` |
| [23](Part%20II%20源码分析/23-RL与训练环境.md) | RL 与训练环境 | `environments/`, `batch_runner.py` |

### Part III 对比分析

代码级对比 Hermes Agent、OpenClaw、EvoMap Evolver 及其他 Harness 实现。

| 章节 | 主题 |
|------|------|
| [24](Part%20III%20对比分析/24-OpenClaw架构对比.md) | OpenClaw 架构对比 — Gateway vs ExecutionLoop |
| [25](Part%20III%20对比分析/25-EvoMap-Evolver对比.md) | EvoMap Evolver 对比 — 自进化模块逐步对比 |
| [26](Part%20III%20对比分析/26-三项目Skill系统对比.md) | 三项目 Skill 系统对比 |
| [27](Part%20III%20对比分析/27-记忆系统三方对比.md) | 记忆系统三方对比 |
| [28](Part%20III%20对比分析/28-开源伦理与启示.md) | 开源伦理与启示 |

### 附录

| 文档 | 内容 |
|------|------|
| [附录 A](Appendix/A-章节配置.yaml) | 章节配置与元数据 |
| [附录 B](Appendix/B-关键数据结构索引.md) | 核心类/函数速查表 |
| [附录 C](Appendix/C-参考文献.md) | 参考文献与引用来源 |

---

## 源码基线

| 项目 | 版本 | 语言 | Stars |
|------|------|------|-------|
| Hermes Agent | v0.9.0 (main, April 2026) | Python | 90,209 |
| OpenClaw | latest (April 2026) | TypeScript | 358,225 |
| EvoMap Evolver | latest (April 2026) | Node.js | 2,512 |

## 怎么读

- **快速了解**：读总纲，15 分钟掌握全貌
- **理解设计**：Part I 从头读，掌握"为什么这么做"
- **看实现**：Part II 按需跳转，追踪代码路径
- **看争议**：直接读 Part III

## 许可

本书内容采用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 许可证。所分析的源码版权归原项目所有者所有。
