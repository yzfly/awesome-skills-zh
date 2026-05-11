# Awesome Claude Skills ZH [![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com) [![License](https://img.shields.io/badge/license-MIT-blue.svg?style=flat-square)](LICENSE)

> 🇨🇳 **中文**：精选 Claude Skills、Agent Skills、LLM Skills 及 AI 智能体开发资源列表。  
> 🇺🇸 **English**: A curated list of awesome Claude Skills, Agent Skills, and AI resources.

---

<div align="center">

**维护者 (Maintainer)**: [云中江树 (yzfly)](https://github.com/yzfly)  
**微信公众号**: 云中江树

<p align="center">
  <a href="#-背景与核心概念-background--concepts">背景概念</a> •
  <a href="#-agent-skill-开放标准-open-standard">开放标准</a> •
  <a href="#-官方文档-official-documentation">官方文档</a> •
  <a href="#-开源-skills-库-open-source-skills">开源库</a> •
  <a href="#-工具与基础设施-tools--infrastructure">工具设施</a> •
  <a href="#-深度文章-articles">深度文章</a>
</p>

</div>

---

## 📖 目录 (Table of Contents)

- [背景与核心概念 (Background & Concepts)](#-背景与核心概念-background--concepts)
  - [什么是 Agent Skills？](#什么是-agent-skills)
  - [核心价值：上下文效率](#核心价值上下文效率-context-efficiency)
  - [架构演进：代码优先](#架构演进代码优先-code-first)
- [Agent Skill 开放标准 (Open Standard)](#-agent-skill-开放标准-open-standard)
- [官方文档 (Official Documentation)](#-官方文档-official-documentation)
- [开源 Skills 库 (Open Source Skills)](#-开源-skills-库-open-source-skills)
- [工具与基础设施 (Tools & Infrastructure)](#-工具与基础设施-tools--infrastructure)
- [精选资源集合 (Awesome Collections)](#-精选资源集合-awesome-collections)
- [深度文章 (Articles)](#-深度文章-articles)
- [关于作者 (About)](#-关于作者-about)
- [Star History](#star-history)

---

## 💡 背景与核心概念 (Background & Concepts)

本章节旨在深入解析 Claude Skills 的技术原理与应用场景，帮助开发者理解为何需要 Skills 以及如何正确构建它。

### 什么是 Agent Skills？

**Agent Skills（智能体技能）** 本质上是关于“**如何做（How-to）**”的知识编码。它不仅仅是提示词，更是智能体的行动指南。

*   **模块化与文件化 (Modularization & Documentation)**  
    在传统的 Prompt Engineering 中，我们往往将大量的指令、示例和约束条件塞入 System Prompt，导致上下文窗口（Context Window）迅速膨胀且难以维护。Anthropic 提出的 Skills 概念，旨在将这些程序性知识**模块化**、**文件化**。

*   **结构化定义 (Structured Definition)**  
    根据 `anthropics/skills` 官方规范，一个 Skill 不仅仅是一段提示词，它是一个包含结构化元数据（YAML Frontmatter）和详细指令（Markdown）的独立单元。
    > **示例**：一个“企业文档编写”的 Skill，不仅包含“语气正式、格式规范”的要求，还可能通过 `SKILL.md` 文件定义了如何调用 Python 脚本来处理 PDF 数据，或者如何验证 Excel 报表的准确性。

### 核心价值：上下文效率 (Context Efficiency)

Skills 的核心价值在于极大地提升了**上下文效率**。与 MCP (Model Context Protocol) 服务器预加载大量工具定义不同，Skills 往往采用“按需加载”或“渐进式披露”的策略。

*   **工作机制**：智能体可能首先读取一个高层级的 Skill 索引，仅在确认为“数据清洗任务”时，才动态加载具体的数据处理 Skill。
*   **类脑机制**：这种机制类似于人类专家在面对特定任务时调取特定的专业记忆，而非时刻保持所有知识在工作记忆中激活。这既节省了 Token，又减少了模型因信息过载产生的幻觉。

### 架构演进：代码优先 (Code-First)

在架构演进中，一个值得注意的趋势是**“代码优先（Code-First）”对纯文本工具调用的替代**。

*   **传统痛点**：传统的 MCP 工具调用（Tool Calling）往往涉及繁琐的 JSON 结构生成。这不仅消耗大量 Token，而且在处理复杂嵌套结构时容易出错。
*   **新范式**：Anthropic 的研究表明，通过让 Agent 编写并执行代码（如 Python 或 Bash 脚本）来调用 MCP 工具，可以显著降低 Token 消耗并提高任务成功率。
    > **示例**：与其让模型生成五个独立的 `read_file` 工具调用 JSON 来读取五个文件，不如让它编写一个 Python `for` 循环来批量读取。
*   **沙盒执行**：这种 **Sandboxed Code Execution（沙盒代码执行）** 模式，结合 MCP 的标准化接口，正在成为构建复杂 Agent 的主流范式。

---

## Agent Skill 开放标准 (Open Standard)

> **最新动态 (2025.12.23 Update)**: Claude 正式开放 Skill 标准，旨在统一智能体技能的描述与交互方式。

*   **官方标准网站**:  
    🌐 [https://agentskills.io](https://agentskills.io)

*   **GitHub 标准仓库**:  
    📂 [https://github.com/agentskills/agentskills](https://github.com/agentskills/agentskills)

#### ⚡️ 快速开发辅助
本仓库提供了 Skills 的标准描述文件，您可以直接复制以下文件内容提供给 AI，方便与 AI 协同开发符合标准的 Skills：
*   📄 **[agentskills.txt](./agentskills.txt)** (点击查看或下载)

---

## 📘 官方文档 (Official Documentation)

Anthropic 官方发布的关于 Agent Skills 的核心指南，是理解技术细节的权威来源。

> **📕 重磅推荐 — Claude 技能构建完全指南 (The Complete Guide to Building Skills for Claude)**
>
> Anthropic 官方出品，从基础概念、规划与设计、测试与迭代、分发与共享到模式与故障排除，系统性地讲解如何构建高质量的 Claude Skill。**强烈建议通读。**
>
> **[英文原版 PDF](./docs/The%20Complete%20Guide%20to%20Building%20Skills%20for%20Claude.pdf)** | **[中文版 PDF](./docs/The-Complete-Guide-to-Building-Skill-for-Claude.no_watermark.zh-CN.pdf)**

| 资源名称 | 描述 |
| :--- | :--- |
| **[用 Agent Skills 为 Agent 赋能](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)** | 📜 **Blog** - 官方博客，深入浅出地介绍了如何利用 Skills 应对真实世界的复杂任务。 |
| **[Agent Skills 开发者指南](https://platform.claude.com/docs/en/build-with-claude/skills-guide)** | 📘 **Guide** - 构建 Skills 的详细技术指南，包含从零开始的步骤。 |
| **[Agent Skills 编写最佳实践](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)** | 🌟 **Best Practices** - 提高 Skills 质量、鲁棒性和可复用性的官方建议。 |
| **[Agent Skills 参考文档](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)** | ⚙️ **API Reference** - 包含详细的技术参数、YAML 格式规范与 API 参考。 |
| **[使用 Claude Agent SDK 构建智能体](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)** | 🛠️ **SDK Tutorial** - 结合官方 SDK 进行开发的实战教程。 |

---

## 📦 开源 Skills 库 (Open Source Skills)

可以直接使用、参考或集成到项目中的 Skill 集合与代码库。

#### 🌟 官方推荐
- [**anthropics/skills**](https://github.com/anthropics/skills)  
  Anthropic 官方维护的 Agent Skills 公共仓库。这是理解标准实现的最佳参考，包含了多个基础且通用的 Skill 范例。

#### 💻 编程与跨平台
- [**andrej-karpathy-skills**](https://github.com/swarmclawai/andrej-karpathy-skills)<br>
  将 Andrej Karpathy 的编码代理建议打包为可安装的跨平台 Skill，支持 Claude Code、Codex、Cursor、Gemini、OpenCode、OpenClaw、AGENTS.md 等智能体环境。

#### 🔬 垂直领域
- [**claude-scientific-skills**](https://github.com/K-Dense-AI/claude-scientific-skills)  
  包含 **125+** 个现成的科学研究类 Skills。专为科研场景设计，涵盖了文献分析、数据处理等专业领域。

#### 🛠️ 生态集成
- [**synalinks-skills**](https://github.com/SynaLinks/synalinks-skills)  
  专为 **Synalinks** 生态系统设计的 Claude Skills 集合，展示了 Skills 在特定框架下的应用。

---

## 🛠️ 工具与基础设施 (Tools & Infrastructure)

用于运行、测试、部署或增强 Claude Skills 的周边工具。

- [**Open Skills**](https://github.com/BandarLabs/open-skills)  
   **跨平台运行器** - 该项目旨在让 Claude Skills 能够本地运行在**任何 LLM** 之上 (OpenSkills: Run Claude Skills Locally using any LLM)，打破了模型的限制。

- [**claude-code-infrastructure-showcase**](https://github.com/diet103/claude-code-infrastructure-showcase)  
   **基建参考** - 一个精选的、经过生产环境测试的 Claude Code 基础设施参考库，为企业级部署提供了宝贵的参考架构。

---

## 📚 精选资源集合 (Awesome Collections)

社区其他维护者整理的相关 Awesome 列表，便于交叉参考。

- [**awesome-claude-skills (TravisVN)**](https://github.com/travisvn/awesome-claude-skills)  
  精选的 Claude Skills、资源和工具列表，内容特别关注 **Claude Code** 的工作流集成。
  
- [**awesome-claude-skills (VoltAgent)**](https://github.com/VoltAgent/awesome-claude-skills)  
  另一个视角下的 Claude Skills 和资源精选合集，包含了不同的工具和案例。

---

## 📝 深度文章 (Articles)

帮助你深入理解技术原理和未来趋势的高质量文章。

- [**Bringing Anthropic Skills to GitHub Copilot**](https://tiberriver256.github.io/ai%20and%20technology/skills-catalog-part-1-indexing-ai-context/)  
  探讨如何将 Anthropic Skills 的概念引入到 GitHub Copilot 及上下文索引中。这篇文章对于理解 Skills 如何跨平台迁移和集成具有很高的参考价值。

---

## 👤 关于作者 (About)

本项目由 **云中江树** 整理维护，致力于推动 AI 智能体技术在中文社区的普及与发展。

- **GitHub**: [@yzfly](https://github.com/yzfly)
- **公众号**: 云中江树

---

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=yzfly/awesome-claude-skills-zh&type=Date)](https://star-history.com/#yzfly/awesome-claude-skills-zh&Date)
