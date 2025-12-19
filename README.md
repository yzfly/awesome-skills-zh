# Awesome Claude Skills ZH [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> 精选 Claude Skills、Agent Skills、LLM Skills 及 AI 智能体开发资源列表。
> A curated list of awesome Claude Skills, Agent Skills, and AI resources.

**维护者**: [云中江树](https://github.com/yzfly)  
**微信公众号**: 云中江树

---

## 📖 目录 (Table of Contents)

- [背景与核心概念](#-背景与核心概念-background--concepts)
- [官方文档](#-官方文档-official-documentation)
- [开源 Skills 库](#-开源-skills-库-open-source-skills)
- [工具与基础设施](#-工具与基础设施-tools--infrastructure)
- [精选资源集合](#-精选资源集合-awesome-collections)
- [深度文章](#-深度文章-articles)
- [关于作者](#-关于作者-about)

---

## 💡 背景与核心概念 (Background & Concepts)

### 什么是 Agent Skills？

**Agent Skills（智能体技能）** 本质上是关于“如何做（How-to）”的知识编码。

*   **模块化与文件化**：在传统的 Prompt Engineering 中，我们往往将大量的指令、示例和约束条件塞入 System Prompt，导致上下文窗口迅速膨胀且难以维护。Anthropic 提出的 Skills 概念，旨在将这些程序性知识模块化、文件化。
*   **结构化定义**：根据 `anthropics/skills` 官方规范，一个 Skill 不仅仅是一段提示词，它是一个包含结构化元数据（YAML Frontmatter）和详细指令（Markdown）的独立单元。
    *   *示例*：一个“企业文档编写”的 Skill，不仅包含“语气正式、格式规范”的要求，还可能通过 `SKILL.md` 文件定义了如何调用 Python 脚本来处理 PDF 数据，或者如何验证 Excel 报表的准确性。

### 核心价值：上下文效率 (Context Efficiency)

Skills 的核心价值在于**上下文效率**。与 MCP 服务器预加载大量工具定义不同，Skills 往往采用“按需加载”或“渐进式披露”的策略。

*   **工作机制**：智能体可能首先读取一个高层级的 Skill 索引，仅在确认为“数据清洗任务”时，才加载具体的数据处理 Skill。
*   **类脑机制**：这种机制类似于人类专家在面对特定任务时调取特定的专业记忆，而非时刻保持所有知识在工作记忆中激活。

### 架构演进：代码优先 (Code-First)

在架构演进中，一个值得注意的趋势是**“代码优先（Code-First）”对纯文本工具调用的替代**。

*   **传统痛点**：传统的 MCP 工具调用（Tool Calling）往往涉及繁琐的 JSON 结构生成，这不仅消耗大量 Token，而且容易出错。
*   **新范式**：Anthropic 的研究表明，通过让 Agent 编写并执行代码（如 Python 或 Bash 脚本）来调用 MCP 工具，可以显著降低 Token 消耗并提高任务成功率。
    *   *示例*：与其让模型生成五个独立的 `read_file` 工具调用来读取五个文件，不如让它编写一个 Python 循环来批量读取。
*   **沙盒执行**：这种 **Sandboxed Code Execution（沙盒代码执行）** 模式，结合 MCP 的标准化接口，正在成为构建复杂 Agent 的主流范式。

---

## 📘 官方文档 (Official Documentation)

Anthropic 官方发布的关于 Agent Skills 的核心指南。

- [**用 Agent Skills 为 Agent 赋能**](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) - 官方博客，介绍如何利用 Skills 应对真实世界任务。
- [**Agent Skills 开发者指南**](https://platform.claude.com/docs/en/build-with-claude/skills-guide) - 构建 Skills 的详细技术指南。
- [**Agent Skills 编写最佳实践**](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) - 提高 Skills 质量和稳定性的官方建议。
- [**Agent Skills 参考文档**](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) - 技术参数与 API 参考。
- [**使用 Claude Agent SDK 构建智能体**](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk) - 结合 SDK 开发的实战教程。

---

## 📦 开源 Skills 库 (Open Source Skills)

可以直接使用的 Skill 集合与代码库。

- [**anthropics/skills**](https://github.com/anthropics/skills)  
  🌟 **Official** | Anthropic 官方的 Agent Skills 公共仓库，标准参考实现。
  
- [**claude-scientific-skills**](https://github.com/K-Dense-AI/claude-scientific-skills)  
  🔬 包含 125+ 个现成的科学研究类 Skills，专为科研场景设计。

- [**synalinks-skills**](https://github.com/SynaLinks/synalinks-skills)  
  🛠️ 专为 Synalinks 生态系统设计的 Claude Skills 集合。

---

## 🛠️ 工具与基础设施 (Tools & Infrastructure)

用于运行、测试或部署 Claude Skills 的工具。

- [**Open Skills**](https://github.com/BandarLabs/open-skills)  
  🚀 让 Claude Skills 可以本地运行在任何 LLM 之上（OpenSkills: Run Claude Skills Locally using any LLM）。

- [**claude-code-infrastructure-showcase**](https://github.com/diet103/claude-code-infrastructure-showcase)  
  🏗️ 一个精选的、经过生产环境测试的 Claude Code 基础设施参考库。

---

## 📚 精选资源集合 (Awesome Collections)

其他社区维护的 Awesome 列表。

- [**awesome-claude-skills (TravisVN)**](https://github.com/travisvn/awesome-claude-skills)  
  精选的 Claude Skills、资源和工具列表，特别关注 Claude Code 工作流。
  
- [**awesome-claude-skills (VoltAgent)**](https://github.com/VoltAgent/awesome-claude-skills)  
  另一个 Claude Skills 和资源的精选合集。

---

## 📝 深度文章 (Articles)

- [**Bringing Anthropic Skills to GitHub Copilot**](https://tiberriver256.github.io/ai%20and%20technology/skills-catalog-part-1-indexing-ai-context/) - 探讨如何将 Anthropic Skills 的概念引入到 GitHub Copilot 及上下文索引中。

---

## 👤 关于作者 (About)

本项目由 **云中江树** 整理维护。

- **GitHub**: [@yzfly](https://github.com/yzfly)

---

**Star History**

[![Star History Chart](https://api.star-history.com/svg?repos=yzfly/awesome-claude-skills-zh&type=Date)](https://star-history.com/#yzfly/awesome-claude-skills-zh&Date)