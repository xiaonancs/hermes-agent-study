# Hermes Agent 深度研究

![Hermes Agent Study Banner](Appendix/cover-banner.png)

覆盖 5 个关键项目：Hermes Agent、OpenClaw、EvoMap Evolver、OpenHarness、JiuwenClaw。
<br>39 篇文章 / 32 章正文 + 1 篇升级专题 / 97 张架构图 / 13,000+ 行研究文本。

> **2026-05 更新**：本研究原以 Hermes Agent v0.9.0（2026-04-13）为基线。v0.9 之后，Hermes 已经连续发布 v0.10 → v0.14 共五个版本（30 天内 ~4,500 commits，~735k 行新增），引入了 Transport ABC、Ink TUI、自治型 Curator、多 Agent Kanban、`/goal` Ralph 循环、PyPI 包、原生 Windows 等大题目。这些变化的总览见专题文章 [近期重大升级 — v0.10 到 v0.14](近期重大升级-v0.10到v0.14.md)；具体落到各章的影响通过每章末尾的"升级补遗"小节同步。

## 系列研究

本研究是 AI Agent / Coding Harness 源码深度研究系列的一部分，姊妹篇：

- [Claude Code 源码解析](https://github.com/xiaonancs/claude-code-source-analysis) — Anthropic Claude Code v2.1.88 完整 Agent Harness 拆解（30 篇 / 23 个子系统 / 179 张架构图）
- [OpenAI Codex 源码深度研究](https://github.com/xiaonancs/codex-source-analysis) — OpenAI Codex CLI + Rust workspace + TS/Python SDK 全栈（27 篇 / ~51 万字 / 149 张 Mermaid 图）
- [OpenClaw 源码深度研究](https://github.com/xiaonancs/openclaw-study) — 358k Stars OpenClaw v2026.4.15 源码 + 23k commits + 生态调研

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

### 总纲与专题

| 文档 | 内容 |
|------|------|
| [总纲 — Hermes Agent 技术主线分析](总纲-Hermes-Agent技术主线分析.md) | 核心机制、火爆原因、设计哲学、架构全貌 |
| [全网调研 — 社区认知地图](全网调研-社区认知地图.md) | 中英文社区技术分析索引、观点争议、认知盲区 |
| [近期重大升级 — v0.10 到 v0.14](近期重大升级-v0.10到v0.14.md) | 30 天内 5 个版本的体系级变更（Transport ABC、Curator、Kanban、`/goal`、22 个平台、PyPI、原生 Windows） |

### Part I 使用方法与原理分析（11 章）

| 序号 | 章节 | 摘要 |
|------|------|------|
| 01 | [项目全景与设计哲学](Part%20I%20Principles%20and%20Usage/01-项目全景与设计哲学.md) | `run_agent.py`, `AGENTS.md` |
| 02 | [启动流程与配置系统](Part%20I%20Principles%20and%20Usage/02-启动流程与配置系统.md) | `hermes_cli/main.py`, `config.py` |
| 03 | [初级使用方法](Part%20I%20Principles%20and%20Usage/03-初级使用方法.md) | 安装、配置、CLI 交互、命令速查 |
| 04 | [高级使用方法](Part%20I%20Principles%20and%20Usage/04-高级使用方法.md) | Cron、Skill、Gateway、MCP、AutoResearch |
| 05 | [Agent 核心循环](Part%20I%20Principles%20and%20Usage/05-Agent核心循环.md) | `run_agent.py` (11,487 行) |
| 06 | [System Prompt 组装](Part%20I%20Principles%20and%20Usage/06-SystemPrompt组装.md) | `agent/prompt_builder.py` |
| 07 | [Provider 与 API 模式](Part%20I%20Principles%20and%20Usage/07-Provider与API模式.md) | `runtime_provider.py`, `auth.py` |
| 08 | [记忆系统](Part%20I%20Principles%20and%20Usage/08-记忆系统.md) | `memory_manager.py`, `memory_tool.py` |
| 09 | [Skill 系统](Part%20I%20Principles%20and%20Usage/09-Skill系统.md) | `skill_commands.py`, `skills_hub.py` |
| 10 | [自进化引擎](Part%20I%20Principles%20and%20Usage/10-自进化引擎.md) | `hermes-agent-self-evolution/` |
| 11 | [状态管理与 Profile 隔离](Part%20I%20Principles%20and%20Usage/11-状态管理与Profile隔离.md) | `hermes_constants.py` |

### Part II 源码分析（14 章）

| 序号 | 章节 | 摘要 |
|------|------|------|
| 12 | [上下文压缩与缓存](Part%20II%20Source%20Analysis/12-上下文压缩与缓存.md) | `context_compressor.py`, `prompt_caching.py` |
| 13 | [会话持久化](Part%20II%20Source%20Analysis/13-会话持久化.md) | `hermes_state.py`, `session.py` |
| 14 | [工具系统总览](Part%20II%20Source%20Analysis/14-工具系统总览.md) | `tools/registry.py`, `toolsets.py` |
| 15 | [终端与沙箱执行](Part%20II%20Source%20Analysis/15-终端与沙箱执行.md) | `terminal_tool.py`, `environments/` |
| 16 | [文件与代码工具](Part%20II%20Source%20Analysis/16-文件与代码工具.md) | `file_tools.py`, `code_execution_tool.py` |
| 17 | [浏览器自动化](Part%20II%20Source%20Analysis/17-浏览器自动化.md) | `browser_tool.py` |
| 18 | [MCP 集成](Part%20II%20Source%20Analysis/18-MCP集成.md) | `mcp_tool.py` |
| 19 | [子 Agent 委托](Part%20II%20Source%20Analysis/19-子Agent委托.md) | `delegate_tool.py` |
| 20 | [定时任务](Part%20II%20Source%20Analysis/20-定时任务.md) | `cron/` |
| 21 | [Gateway 网关](Part%20II%20Source%20Analysis/21-Gateway网关.md) | `gateway/run.py` (9,798 行) |
| 22 | [插件系统](Part%20II%20Source%20Analysis/22-插件系统.md) | `plugins.py`, `plugins/` |
| 23 | [ACP IDE 集成](Part%20II%20Source%20Analysis/23-ACP-IDE集成.md) | `acp_adapter/` |
| 24 | [安全与权限](Part%20II%20Source%20Analysis/24-安全与权限.md) | `approval.py`, `skills_guard.py` |
| 25 | [RL 与训练环境](Part%20II%20Source%20Analysis/25-RL与训练环境.md) | `environments/`, `batch_runner.py` |

### Part III 同源项目对比分析（7 章）

| 序号 | 章节 | 摘要 |
|------|------|------|
| 26 | [OpenClaw 架构对比](Part%20III%20Comparative%20Analysis/26-OpenClaw架构对比.md) | Gateway vs ExecutionLoop |
| 27 | [EvoMap Evolver 对比](Part%20III%20Comparative%20Analysis/27-EvoMap-Evolver对比.md) | 自进化模块逐步对比 |
| 28 | [OpenHarness 与 JiuwenClaw 对比](Part%20III%20Comparative%20Analysis/28-OpenHarness与JiuwenClaw对比.md) | 三种 Agent 哲学 |
| 29 | [全网 Harness 实现全景](Part%20III%20Comparative%20Analysis/29-全网Harness实现全景.md) | 四种范式分类学 |
| 30 | [多项目 Skill 系统对比](Part%20III%20Comparative%20Analysis/30-三项目Skill系统对比.md) | 五项目横评 |
| 31 | [多项目记忆系统对比](Part%20III%20Comparative%20Analysis/31-记忆系统三方对比.md) | 五项目横评 |
| 32 | [开源伦理与启示](Part%20III%20Comparative%20Analysis/32-开源伦理与启示.md) | 许可、引用、社区规范 |

### 附录

| 文档 | 内容 |
|------|------|
| [附录 A](Appendix/A-章节配置.yaml) | 章节配置与元数据 |
| [附录 B](Appendix/B-关键数据结构索引.md) | 核心类/函数速查表 |
| [附录 C](Appendix/C-参考文献.md) | 参考文献与引用来源 |

---

## 源码基线

| 项目 | 主线研究基线版本 | 升级追加版本 | 语言 | Stars |
|------|------------------|---------------|------|-------|
| Hermes Agent | v0.9.0（2026-04-13） | v0.14.0（2026-05-16，main HEAD 2026-05-25） | Python | 90,209+ |
| OpenClaw | latest (April 2026) | — | TypeScript | 358,225 |
| EvoMap Evolver | latest (April 2026) | — | Node.js | 2,512 |
| OpenHarness | v0.1.6 (April 2026) | — | Python | 9,884 |
| JiuwenClaw | latest (April 2026) | — | Python | 398 |

> v0.9.0 → v0.14.0 的源码规模：Python 文件 886 → 1,898（+114%）；Python 行数 411k → 912k（+122%）；消息平台 18 → 22；可选 Skill 子目录 14 → 82。

## 怎么读

- **快速了解**：读总纲，15 分钟掌握全貌
- **看近期升级**：读 [近期重大升级 — v0.10 到 v0.14](近期重大升级-v0.10到v0.14.md)，理解 v0.9 之后的体系级变化（Transport ABC、Curator、Kanban、`/goal`、PyPI、原生 Windows 等）
- **快速上手**：Part I 第 3-4 章，从安装到高级用法
- **研究型用法**：Part I 第 4 章先看高级场景定位与 `autoresearch` 延展，再接 Part II 第 25 章理解 Batch / RL 路径
- **自动研究工作流**：先读 Part I 第 4 章的 `autoresearch` 小节，再按需要跳到 Part II 第 20 章 `定时任务` 和第 25 章 `RL 与训练环境`
- **理解设计**：Part I 从头读，掌握"为什么这么做"
- **看实现**：Part II 按需跳转，追踪代码路径；变化大的章节末尾有"升级补遗"小节
- **看争议**：直接读 Part III

## 许可

本研究内容采用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 许可证。所分析的源码版权归原项目所有者所有。
