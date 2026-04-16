# Hermes Agent 深度研究

![Hermes Agent Study Banner](Appendix/cover-banner.png)

覆盖 5 个关键项目：Hermes Agent、OpenClaw、EvoMap Evolver、OpenHarness、JiuwenClaw。
<br>38 篇文章 / 32 章正文 / 97 张架构图 / 11,000+ 行研究文本。


## 引言

Hermes Agent 是 2026 年最受关注的开源 AI Agent 项目之一——90k Stars，Nous Research 出品，口号是"The agent that grows with you"。

但它的火爆背后有两个绕不开的问题：

1. 整体架构（Gateway、Tool Registry、Skill System、启动文件体系）与 OpenClaw（358k Stars）存在大量可比结构，只是从 TypeScript 换成了 Python，还内置了 `hermes claw migrate` 迁移工具。
2. 核心差异化卖点"Self-evolve"被 EvoMap 指控架构级抄袭——10 步主循环对齐、12 组术语替换、三层记忆对应、7 份材料零引用。

本研究对 Hermes Agent 从三部分进行源码层面拆解：
 <br>`Part I 使用方法与原理分析（11 章）`
 <br>`Part II 源码解析（14 章）`
 <br>`Part III 同源项目对比分析（7 章）`
 <br>同时也针对性补充了近来社区讨论迅速升温的话题：`AutoResearch`。


## 研究结构

### 总纲

| 文档 | 内容 |
|------|------|
| [总纲 — Hermes Agent 技术主线分析](总纲-Hermes-Agent技术主线分析.md) | 核心机制、火爆原因、设计哲学、架构全貌 |
| [全网调研 — 社区认知地图](全网调研-社区认知地图.md) | 中英文社区技术分析索引、观点争议、认知盲区 |

### Part I 使用方法与原理分析（11 章）

| 章节 | 主题 | 核心源码 |
|------|------|---------|
| [01](Part%20I%20Principles%20and%20Usage/01-项目全景与设计哲学.md) | 项目全景与设计哲学 | `run_agent.py`, `AGENTS.md` |
| [02](Part%20I%20Principles%20and%20Usage/02-启动流程与配置系统.md) | 启动流程与配置系统 | `hermes_cli/main.py`, `config.py` |
| [03](Part%20I%20Principles%20and%20Usage/03-初级使用方法.md) | 初级使用方法 | 安装、配置、CLI 交互、命令速查 |
| [04](Part%20I%20Principles%20and%20Usage/04-高级使用方法.md) | 高级使用方法 | Cron 自动化、Skill 生态、Gateway 部署、MCP 集成、Batch 与 AutoResearch 使用定位 |
| [05](Part%20I%20Principles%20and%20Usage/05-Agent核心循环.md) | Agent 核心循环 | `run_agent.py` (11,487 行) |
| [06](Part%20I%20Principles%20and%20Usage/06-SystemPrompt组装.md) | System Prompt 组装 | `agent/prompt_builder.py` |
| [07](Part%20I%20Principles%20and%20Usage/07-Provider与API模式.md) | Provider 与 API 模式 | `runtime_provider.py`, `auth.py` |
| [08](Part%20I%20Principles%20and%20Usage/08-记忆系统.md) | 记忆系统 | `memory_manager.py`, `memory_tool.py` |
| [09](Part%20I%20Principles%20and%20Usage/09-Skill系统.md) | Skill 系统 | `skill_commands.py`, `skills_hub.py` |
| [10](Part%20I%20Principles%20and%20Usage/10-自进化引擎.md) | 自进化引擎 | `hermes-agent-self-evolution/` |
| [11](Part%20I%20Principles%20and%20Usage/11-状态管理与Profile隔离.md) | 状态管理与 Profile 隔离 | `hermes_constants.py` |

### Part II 源码分析（14 章）

| 章节 | 主题 | 核心源码 |
|------|------|---------|
| [12](Part%20II%20Source%20Analysis/12-上下文压缩与缓存.md) | 上下文压缩与缓存 | `context_compressor.py`, `prompt_caching.py` |
| [13](Part%20II%20Source%20Analysis/13-会话持久化.md) | 会话持久化 | `hermes_state.py`, `session.py` |
| [14](Part%20II%20Source%20Analysis/14-工具系统总览.md) | 工具系统总览 | `tools/registry.py`, `toolsets.py` |
| [15](Part%20II%20Source%20Analysis/15-终端与沙箱执行.md) | 终端与沙箱执行 | `terminal_tool.py`, `environments/` |
| [16](Part%20II%20Source%20Analysis/16-文件与代码工具.md) | 文件与代码工具 | `file_tools.py`, `code_execution_tool.py` |
| [17](Part%20II%20Source%20Analysis/17-浏览器自动化.md) | 浏览器自动化 | `browser_tool.py` |
| [18](Part%20II%20Source%20Analysis/18-MCP集成.md) | MCP 集成 | `mcp_tool.py` |
| [19](Part%20II%20Source%20Analysis/19-子Agent委托.md) | 子 Agent 委托 | `delegate_tool.py` |
| [20](Part%20II%20Source%20Analysis/20-定时任务.md) | 定时任务 | `cron/` |
| [21](Part%20II%20Source%20Analysis/21-Gateway网关.md) | Gateway 网关 | `gateway/run.py` (9,798 行) |
| [22](Part%20II%20Source%20Analysis/22-插件系统.md) | 插件系统 | `plugins.py`, `plugins/` |
| [23](Part%20II%20Source%20Analysis/23-ACP-IDE集成.md) | ACP IDE 集成 | `acp_adapter/` |
| [24](Part%20II%20Source%20Analysis/24-安全与权限.md) | 安全与权限 | `approval.py`, `skills_guard.py` |
| [25](Part%20II%20Source%20Analysis/25-RL与训练环境.md) | RL 与训练环境 | `environments/`, `batch_runner.py` |

### Part III 同源项目对比分析（7 章）

| 章节 | 主题 |
|------|------|
| [26](Part%20III%20Comparative%20Analysis/26-OpenClaw架构对比.md) | OpenClaw 架构对比 — Gateway vs ExecutionLoop |
| [27](Part%20III%20Comparative%20Analysis/27-EvoMap-Evolver对比.md) | EvoMap Evolver 对比 — 自进化模块逐步对比 |
| [28](Part%20III%20Comparative%20Analysis/28-OpenHarness与JiuwenClaw对比.md) | OpenHarness 与 JiuwenClaw 对比 — 三种 Agent 哲学 |
| [29](Part%20III%20Comparative%20Analysis/29-全网Harness实现全景.md) | 全网 Harness 实现全景 — 四种范式分类学 |
| [30](Part%20III%20Comparative%20Analysis/30-三项目Skill系统对比.md) | 多项目 Skill 系统对比 |
| [31](Part%20III%20Comparative%20Analysis/31-记忆系统三方对比.md) | 多项目记忆系统对比 |
| [32](Part%20III%20Comparative%20Analysis/32-开源伦理与启示.md) | 开源伦理与启示 |

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
| OpenHarness | v0.1.6 (April 2026) | Python | 9,884 |
| JiuwenClaw | latest (April 2026) | Python | 398 |

## 怎么读

- **快速了解**：读总纲，15 分钟掌握全貌
- **快速上手**：Part I 第 3-4 章，从安装到高级用法
- **研究型用法**：Part I 第 4 章先看高级场景定位与 `autoresearch` 延展，再接 Part II 第 25 章理解 Batch / RL 路径
- **自动研究工作流**：先读 Part I 第 4 章的 `autoresearch` 小节，再按需要跳到 Part II 第 20 章 `定时任务` 和第 25 章 `RL 与训练环境`
- **理解设计**：Part I 从头读，掌握"为什么这么做"
- **看实现**：Part II 按需跳转，追踪代码路径
- **看争议**：直接读 Part III

## 许可

本研究内容采用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 许可证。所分析的源码版权归原项目所有者所有。
