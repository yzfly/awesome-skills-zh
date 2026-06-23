# Awesome Claude Skills ZH [![Awesome](https://awesome.re/badge.svg)](https://awesome.re) [![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com) [![License](https://img.shields.io/badge/license-Apache--2.0-blue.svg?style=flat-square)](LICENSE)

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
  <a href="#-明星单项-skills-featured-standalone-skills">明星 Skills</a> •
  <a href="#-安全与逆向-security--reverse-engineering">安全逆向</a> •
  <a href="#-女娲--人物思维-skill-生态-nuwa--persona-skills">女娲生态</a> •
  <a href="#-中文社区-skills-chinese-community">中文社区</a> •
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
- [明星单项 Skills (Featured Standalone Skills)](#-明星单项-skills-featured-standalone-skills)
- [安全与逆向 (Security & Reverse Engineering)](#-安全与逆向-security--reverse-engineering)
- [女娲 · 人物思维 Skill 生态 (Nuwa / Persona Skills)](#-女娲--人物思维-skill-生态-nuwa--persona-skills)
- [中文社区 Skills (Chinese Community)](#-中文社区-skills-chinese-community)
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

> **最新动态 (2025.12.18 Update)**: Claude 正式开放 Skill 标准，旨在统一智能体技能的描述与交互方式。

> **重要背景**: Agent Skills 开放标准已捐赠给 Linux Foundation 旗下的 **Agentic AI Foundation (AAIF)**，由中立的开放治理机构推动其演进，确保标准不被单一厂商绑定。

*   **AAIF 基金会**:  
    🌐 [https://aaif.io](https://aaif.io)

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
| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**anthropics/skills**](https://github.com/anthropics/skills) | `147k` | Anthropic 官方维护的 Agent Skills 公共仓库，理解标准实现的最佳参考，含多个通用 Skill 范例。 |
| [**anthropics/claude-cookbooks**](https://github.com/anthropics/claude-cookbooks/tree/main/skills) | `45k` | 官方 Cookbook 的 Skills 目录，提供可直接运行的端到端示例与教程，适合上手实践。 |

#### 🚀 社区框架
| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**obra/superpowers**](https://github.com/obra/superpowers) | `219k` | 2026 年最具影响力的社区 Skills 框架，已入选官方插件市场，提供一整套可组合的高级 Skill 与工作流。 |
| [**obra/superpowers-marketplace**](https://github.com/obra/superpowers-marketplace) | `1.0k` | Superpowers 配套的 Skills 市场，便于发现、安装与分享社区贡献的 Skills。 |

#### 📚 大型 Skills 合集
| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**mattpocock/skills**](https://github.com/mattpocock/skills) | `142k` | TypeScript 知名教育者 Matt Pocock 个人 `.claude` 目录中的工程实战 Skills，小而易改、可组合、与模型无关，覆盖 `/tdd`、`/grill-me`、架构改进、领域建模、调试等软件工程基本功。 |
| [**addyosmani/agent-skills**](https://github.com/addyosmani/agent-skills) | `58.9k` | Addy Osmani 出品的「生产级」工程 Skills 合集，面向 AI 编程智能体，覆盖前端、性能、调试等高质量工程实践。 |
| [**vercel-labs/agent-skills**](https://github.com/vercel-labs/agent-skills) | `27.9k` | Vercel 官方出品的 Agent Skills 合集，面向 Next.js、Vercel 平台与现代前端工程实践。 |
| [**alirezarezvani/claude-skills**](https://github.com/alirezarezvani/claude-skills) | `17.3k` | 社区最大规模合集之一，收录 **337** 个 skills / agent skills / 插件，含 30+ Agents、70+ 自定义命令。 |
| [**muratcankoylan/Agent-Skills-for-Context-Engineering**](https://github.com/muratcankoylan/Agent-Skills-for-Context-Engineering) | `16.6k` | 面向上下文工程（Context Engineering）与多智能体架构的 Agent Skills 合集，聚焦上下文管理与协作编排实践。 |
| [**Jeffallan/claude-skills**](https://github.com/Jeffallan/claude-skills) | `9.7k` | 面向全栈开发者的 **66** 个专精 Skills，将 Claude Code 打造成全栈开发搭档。 |
| [**mrgoonie/claudekit-skills**](https://github.com/mrgoonie/claudekit-skills) | `2.1k` | ClaudeKit.cc 出品的全套高质量 Skills 合集。 |
| [**simonw/claude-skills**](https://github.com/simonw/claude-skills) | `0.9k` | Simon Willison 整理的 `/mnt/skills` 目录真实内容，适合研究官方内置 Skill 的实现。 |
| [**mohitagw15856/pm-claude-skills**](https://github.com/mohitagw15856/pm-claude-skills) | `0.9k` | 覆盖 17 个职业方向的 **167** 个专业级 Skills，主打产品 / 项目管理与办公效率。 |
| [**coleam00/second-brain-skills**](https://github.com/coleam00/second-brain-skills) | `0.8k` | 将 Claude Code 变成「第二大脑」的合集，主打知识管理与个人信息检索。 |
| [**jezweb/claude-skills**](https://github.com/jezweb/claude-skills) | `0.8k` | 面向 Claude Code CLI 的全栈开发 Skills，涵盖 Cloudflare、React、Tailwind v4。 |

#### 🔬 垂直领域
| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**K-Dense-AI/scientific-agent-skills**](https://github.com/K-Dense-AI/scientific-agent-skills) | `27.4k` | **125+** 个科学研究类 Skills，专为科研设计，涵盖文献分析、数据处理等领域。 |
| [**nowork-studio/NotFair**](https://github.com/nowork-studio/NotFair) | `2.8k` | 开源营销增长 Skills，覆盖 SEO、GEO、Google Ads、Meta Ads 等投放场景。 |
| [**dominikmartn/nothing-design-skill**](https://github.com/dominikmartn/nothing-design-skill) | `2.4k` | 以 Nothing 设计语言（单色、点阵风）生成 UI 的 Skill。 |
| [**samber/cc-skills-golang**](https://github.com/samber/cc-skills-golang) | `2.2k` | 一套实际可用的 Golang agentic skills 合集，面向 Go 工程实践。 |
| [**aaron-he-zhu/seo-geo-claude-skills**](https://github.com/aaron-he-zhu/seo-geo-claude-skills) | `2.0k` | 20 个 SEO 与 GEO（生成式引擎优化）Skills，兼容 35+ AI 智能体。 |
| [**tradermonty/claude-trading-skills**](https://github.com/tradermonty/claude-trading-skills) | `1.8k` | 面向股票投资者与交易员的 Skills，提供市场分析、技术指标等能力。 |
| [**jherrodthomas/automotive-skills-suite**](https://github.com/jherrodthomas/automotive-skills-suite) | `1.6k` | **100+** 个汽车工程领域 Skills，覆盖 ISO 26262 功能安全等专业场景。 |
| [**Sushegaad/Claude-Skills-Governance-Risk-and-Compliance**](https://github.com/Sushegaad/Claude-Skills-Governance-Risk-and-Compliance) | `0.6k` | 治理、风险与合规（GRC）专家级 Skills，覆盖 ISO 27001、SOC 2、GDPR、HIPAA、NIST、EU AI Act 等数十种标准。 |
| [**jherrodthomas/robotics-skills-suite**](https://github.com/jherrodthomas/robotics-skills-suite) | `0.4k` | **76** 个可审计的机器人领域 Skills，覆盖工业机器人、协作机器人、AMR、ROS2 与 IEC 62443 全生命周期。 |
| [**posit-dev/skills**](https://github.com/posit-dev/skills) | `0.4k` | Posit（原 RStudio）官方出品的 Claude Skills 合集，面向 R 与数据科学。 |
| [**palkan/skills**](https://github.com/palkan/skills) | `0.4k` | 基于《Layered Rails》一书提炼的 Rails 开发 Skills，作者为 Evil Martians 的 palkan。 |
| [**jamditis/claude-skills-journalism**](https://github.com/jamditis/claude-skills-journalism) | `0.3k` | 面向新闻、媒体与学术的 Skills：事实核查、FOIA 信息公开、数据新闻、学术写作等。 |
| [**haowjy/creative-writing-skills**](https://github.com/haowjy/creative-writing-skills) | `0.2k` | 专注创意写作的 Claude skills 合集。 |
| [**ahmedasmar/devops-claude-skills**](https://github.com/ahmedasmar/devops-claude-skills) | `0.2k` | 面向 DevOps 工作流的 Claude Code Skills 市场。 |
| [**yzfly/awesome-design-html**](https://github.com/yzfly/awesome-design-html) | `0.1k` | **115** 个品牌主题 HTML 设计（93 网页 + 22 iOS，含 20 个中国品牌）打包成的 Claude Code skill，一行安装后直接对话「做一个飞书风的页面」（本仓库维护者 [@yzfly](https://github.com/yzfly) 出品）。 |

#### 🛠️ 生态集成
| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**googleworkspace/cli**](https://github.com/googleworkspace/cli) | `27.2k` | Google Workspace 官方 CLI（`gws`），统一访问 Drive/Gmail/Calendar/Sheets/Docs/Chat 等 API；内置 **100+ Agent Skills（`SKILL.md`）**，覆盖每个 API 及常用工作流与 50 个精选配方，让 LLM 无需自定义工具即可操作 Workspace。 |
| [**kepano/obsidian-skills**](https://github.com/kepano/obsidian-skills) | `35.6k` | Obsidian 作者 [@kepano](https://github.com/kepano) 出品，让智能体通过 Obsidian CLI 与开放文件格式操作笔记库的 Skills 合集。 |
| [**SynaLinks/synalinks-skills**](https://github.com/SynaLinks/synalinks-skills) | `0.9k` | 专为 **Synalinks** 生态系统设计的 Claude Skills 集合，展示特定框架下的应用。 |

---

## ⭐ 明星单项 Skills (Featured Standalone Skills)

GitHub 上 Star 数最高、最具话题度的单一用途 Skill。它们大多只做一件事，却把这件事做到极致，是学习「一个好 Skill 该长什么样」的绝佳范例。

| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**multica-ai/andrej-karpathy-skills**](https://github.com/multica-ai/andrej-karpathy-skills) | `168.9k` | **Star 数最高的单项 Skill（单个 `CLAUDE.md` 文件）**，由华人开发者 **Forrest Chang** 维护，提炼 Andrej Karpathy 的编程理念以改善 Claude Code 行为。中文版见 [LearnPrompt/andrej-karpathy-skills](https://github.com/LearnPrompt/andrej-karpathy-skills)。 |
| [**JuliusBrussee/caveman**](https://github.com/JuliusBrussee/caveman) | `69.3k` | *"why use many token when few token do trick"*——通过精简表达大幅削减 token 消耗。 |
| [**mvanhorn/last30days-skill**](https://github.com/mvanhorn/last30days-skill) | `41.5k` | 跨 Reddit / X / YouTube / Hacker News 等平台检索近 30 天动态，汇总成一份综合摘要。 |
| [**OthmanAdi/planning-with-files**](https://github.com/OthmanAdi/planning-with-files) | `22.8k` | Manus 风格「持久化 Markdown 规划」，让 Claude 把任务计划落盘成文件，长任务不丢上下文。 |
| [**blader/humanizer**](https://github.com/blader/humanizer) | `22.6k` | 去除文本中「AI 味」痕迹，让 Claude 生成的文字更自然、更像人写的。 |
| [**nidhinjs/prompt-master**](https://github.com/nidhinjs/prompt-master) | `8.9k` | 为任意 AI 工具自动撰写精准提示词，零 token 调用外部 API。 |
| [**SawyerHood/dev-browser**](https://github.com/SawyerHood/dev-browser) | `6.2k` | 赋予智能体使用网页浏览器的能力，让 Claude 真正「上网」操作。 |
| [**uditgoenka/autoresearch**](https://github.com/uditgoenka/autoresearch) | `5.1k` | 受 Karpathy autoresearch 启发的 Claude 自主研究 Skill：以「修改 → 验证 → 保留 / 丢弃 → 循环」的目标驱动迭代，让 Claude Code 自动收敛到目标。 |
| [**zarazhangrui/codebase-to-course**](https://github.com/zarazhangrui/codebase-to-course) | `4.6k` | 将任意代码库转化为精美、可交互的教程，适合技术布道与上手文档。 |
| [**virgiliojr94/book-to-skill**](https://github.com/virgiliojr94/book-to-skill) | `4.4k` | 把任意技术书籍 PDF 转化为可学习、可引用的 Claude Code skill。 |
| [**lackeyjb/playwright-skill**](https://github.com/lackeyjb/playwright-skill) | `2.7k` | 基于 Playwright 的浏览器自动化 Skill，模型可按需自动调用完成网页操作。 |
| [**dgreenheck/webgpu-claude-skill**](https://github.com/dgreenheck/webgpu-claude-skill) | `1.0k` | 用于 Three.js + WebGPU 应用开发的 Skill。 |
| [**zarazhangrui/youtube-to-ebook**](https://github.com/zarazhangrui/youtube-to-ebook) | `0.5k` | 把喜欢频道的 YouTube 字幕定期转成 EPUB 电子书并投递到邮箱。 |
| [**mrtooher/fable-mode**](https://github.com/mrtooher/fable-mode) | `0.5k` | 激活 Fable 式 agentic 行为的 Claude Skill：显式多阶段规划、子 agent 委派与自我验证。 |
| [**Gabberflast/academic-pptx-skill**](https://github.com/Gabberflast/academic-pptx-skill) | `0.5k` | 生成学术演示文稿（会议演讲、研讨、答辩、基金汇报），强制行动式标题、论证结构与引用规范。 |
| [**aiwithremy/claude-skills-llm-council**](https://github.com/aiwithremy/claude-skills-llm-council) | `0.4k` | 「LLM 议会」：让你的决策经过 5 位 AI 顾问的同行评审后再给出结论。 |
| [**alonw0/web-asset-generator**](https://github.com/alonw0/web-asset-generator) | `0.4k` | 从 logo、文字或 emoji 生成 favicon、App 图标与社交媒体配图，支持框架自动集成。 |
| [**coffeefuelbump/csv-data-summarizer-claude-skill**](https://github.com/coffeefuelbump/csv-data-summarizer-claude-skill) | `0.4k` | 上传 CSV 自动用 pandas 生成统计摘要、检测缺失值并产出可视化。 |
| [**ItsssssJack/power-design**](https://github.com/ItsssssJack/power-design) | `0.4k` | 让幻灯片「不像 AI 做的」：品牌 DNA × 20 条设计原则的演示设计 Skill。 |
| [**zippoxer/subtask**](https://github.com/zippoxer/subtask) | `0.3k` | 在独立 Git worktree 中调度子智能体并行完成任务的 Skill。 |
| [**keli-wen/agentic-harness-patterns-skill**](https://github.com/keli-wen/agentic-harness-patterns-skill) | `0.3k` | harness 工程 Agent Skill：记忆、权限、上下文工程与多 agent 协调，从 Claude Code 提炼，中英双语。 |
| [**chrisvoncsefalvay/claude-d3js-skill**](https://github.com/chrisvoncsefalvay/claude-d3js-skill) | `0.2k` | 专注 d3.js 数据可视化开发的 Skill。 |
| [**blader/theorist**](https://github.com/blader/theorist) | `0.2k` | 由 humanizer 作者出品，为每个仓库维护一份「运行理论（operating theory）」文档的 Codex/Claude skill。 |

---

## 🔒 安全与逆向 (Security & Reverse Engineering)

面向安全研究、渗透测试、漏洞挖掘与逆向工程的 Skills。请仅在获得授权的合法测试与研究场景下使用。

| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**SimoneAvogadro/android-reverse-engineering-skill**](https://github.com/SimoneAvogadro/android-reverse-engineering-skill) | `6.0k` | 辅助 Android App 逆向工程的 Claude Code skill。 |
| [**trailofbits/skills**](https://github.com/trailofbits/skills) | `5.6k` | 知名安全公司 **Trail of Bits** 出品，面向安全研究、漏洞检测的 Skills 合集。 |
| [**NVIDIA/SkillSpector**](https://github.com/NVIDIA/SkillSpector) | `4.7k` | NVIDIA 出品的 Agent Skills 安全扫描器，检测 Skills 中的漏洞、恶意模式与安全风险。 |
| [**SnailSploit/Claude-Red**](https://github.com/SnailSploit/Claude-Red) | `2.1k` | 面向红队 / 攻击性安全的精选 Skill 库。 |
| [**elementalsouls/Claude-BugHunter**](https://github.com/elementalsouls/Claude-BugHunter) | `1.7k` | 用于漏洞挖掘与外部红队作业的 Skill bundle，含 71+ 模块。 |
| [**elementalsouls/Claude-OSINT**](https://github.com/elementalsouls/Claude-OSINT) | `1.6k` | 两个配套的 OSINT 情报搜集 Skill，含 90+ 侦察模块、48 个密钥正则与 80+ 数据源。 |
| [**BrownFineSecurity/iothackbot**](https://github.com/BrownFineSecurity/iothackbot) | `0.8k` | 面向 IoT 渗透测试的 Claude Skills 与定制工具集合。 |
| [**Eyadkelleh/awesome-claude-skills-security**](https://github.com/Eyadkelleh/awesome-claude-skills-security) | `0.3k` | 面向授权渗透测试、CTF 与漏洞赏金的安全工具包：精选 SecLists 字典、注入 payload 与专家级 agent。 |

---

## 🧬 女娲 · 人物思维 Skill 生态 (Nuwa / Persona Skills)

由华人开发者 **花叔 ([@alchaincyf](https://github.com/alchaincyf))** 发起的「人物认知操作系统」Skill 生态，是 2026 年中文社区最具影响力的现象级 Skill 玩法——**蒸馏任何人的思维方式**（心智模型、决策启发式、表达 DNA），让 Claude 以特定人物的思维框架运行。

| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**alchaincyf/nuwa-skill**](https://github.com/alchaincyf/nuwa-skill) (女娲.skill) | `22.9k` | 🔧 生态本体。输入任意人物，自动生成一个可运行的人物思维 Skill，下方多数人物 Skill 均由它生成。 |
| [**alchaincyf/zhangxuefeng-skill**](https://github.com/alchaincyf/zhangxuefeng-skill) (张雪峰.skill) | `7.6k` | 高考志愿 / 考研 / 职业规划的实战思维框架。 |
| [**alchaincyf/darwin-skill**](https://github.com/alchaincyf/darwin-skill) (达尔文.skill) | `3.5k` | 🔧 让 Skill 无限进化：评估 → 改进 → 测试 → 保留或回滚的自我迭代机制。 |
| [**tmstack/awesome-persona-skills**](https://github.com/tmstack/awesome-persona-skills) | `2.6k` | 📚 同事.skill、老板.skill、前任.skill、永生.skill…… 人物 Skill 大合集。 |
| [**Panmax/awesome-nuwa**](https://github.com/Panmax/awesome-nuwa) | `0.1k` | 📚 用女娲蒸馏的人物思维框架合集。 |

> 此外，作者还用女娲生成了乔布斯、马斯克、芒格、特朗普、Karpathy、纳瓦尔、费曼等多位人物的 `.skill`（均在 1k star 以下），完整清单见上方 awesome 列表与 [作者主页](https://github.com/alchaincyf?tab=repositories)。

---

## 🇨🇳 中文社区 Skills (Chinese Community)

由中文社区作者开发、文档以中文为主或专为中文场景优化的 Skills，对国内用户尤为友好。

| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**alchaincyf/huashu-design**](https://github.com/alchaincyf/huashu-design) (花叔设计) | `16.4k` | 花叔出品的 HTML-native 设计 Skill，让 Claude 直接产出高质量网页设计。 |
| [**op7418/Humanizer-zh**](https://github.com/op7418/Humanizer-zh) | `9.3k` | [blader/humanizer](https://github.com/blader/humanizer) 的汉化版，专门消除中文文本中的 AI 生成痕迹。 |
| [**joeseesun/qiaomu-anything-to-notebooklm**](https://github.com/joeseesun/qiaomu-anything-to-notebooklm) | `4.9k` | 面向 NotebookLM 的多源内容处理 Skill，支持微信公众号文章等来源的采集与整理。 |
| [**Ceeon/videocut-skills**](https://github.com/Ceeon/videocut-skills) | `1.8k` | 用 Claude Code Skills 打造的视频剪辑 Agent。 |
| [**huangserva/skill-prompt-generator**](https://github.com/huangserva/skill-prompt-generator) | `1.4k` | 基于 Claude Skill 的 AI 人像 Prompt 生成系统，可从特征库智能组合并自动学习扩展。 |
| [**P4nda0s/reverse-skills**](https://github.com/P4nda0s/reverse-skills) | `1.2k` | 逆向工程 Claude Code Skills 插件，中文文档友好。 |
| [**yzfly/douyin-mcp-server**](https://github.com/yzfly/douyin-mcp-server) | `1.0k` | 提取抖音无水印视频链接与文案，同时支持 MCP 与 Claude Skill（本仓库维护者 [@yzfly](https://github.com/yzfly) 出品）。 |
| [**alchaincyf/huashu-skills**](https://github.com/alchaincyf/huashu-skills) (花叔内容创作) | `0.9k` | 花叔的内容创作 Skills 合集，含 AI 审校、选题生成、视频大纲、素材搜索等 11 个技能。 |
| [**jwangkun/claude-for-financial-services-cn**](https://github.com/jwangkun/claude-for-financial-services-cn) | `0.5k` | 面向 A 股金融从业者的 63 个 Claude Skills，基于 Anthropic 官方 claude-for-financial-services 深度适配国内市场。 |
| [**LeastBit/Claude_skills_zh-CN**](https://github.com/LeastBit/Claude_skills_zh-CN) | `0.5k` | Anthropic 官方 `anthropics/skills` 仓库的中文学习版，逐个 Skill 翻译讲解，适合中文用户上手官方范例。 |
| [**chubbyguan/chubbyskills**](https://github.com/chubbyguan/chubbyskills) | `0.4k` | 把抖音 / B 站 / 小红书 / 公众号 / X / 播客等中文全渠道内容采集进个人知识库的 13 个 AI Skill，字幕优先免 GPU，附知识库 MCP server。 |
| [**op7418/Video-Wrapper-Skills**](https://github.com/op7418/Video-Wrapper-Skills) | `0.3k` | 归藏（op7418）出品，为访谈视频自动添加综艺风格视觉特效：AI 分析字幕生成建议，用户审批后自动渲染。 |
| [**liangdabiao/amazon-sorftime-research-MCP-skill**](https://github.com/liangdabiao/amazon-sorftime-research-MCP-skill) | `0.3k` | 基于 Sorftime MCP 的亚马逊选品分析 Skill，覆盖 Listing 全维度穿透、全品类分析、关键词与差评分析等竞品调研。 |
| [**chenxiachan/xhs-claude-skills**](https://github.com/chenxiachan/xhs-claude-skills) | `0.3k` | 把小红书笔记一键提取整理进 Obsidian 的 Claude Code 斜杠命令集。 |
| [**kangarooking/x-skills**](https://github.com/kangarooking/x-skills) | `0.3k` | 自动收集素材、确认选题、创作并发布 X（推特）推文到草稿箱的 Skills 工作流。 |
| [**cclank/lanshu-awesome-ai-video-kit**](https://github.com/cclank/lanshu-awesome-ai-video-kit) | `0.3k` | 「蓝鼠」企业 AI 视频实战工具包：411 个 prompt、15 个模型、7 个 Claude Skill 与 14 篇方法论。 |
| [**leemysw/feishu-docx**](https://github.com/leemysw/feishu-docx) | `0.2k` | 飞书 / Lark 文档、表格、多维表与 Markdown 互转，AI Agent 友好，支持 Claude Skills 调用。 |
| [**sanshao85/claude-skills-guide**](https://github.com/sanshao85/claude-skills-guide) | `0.2k` | 《Claude Skills 开发完全指南》，从基础到精通的中文系统教程。 |
| [**YANZHANLIN/ielts-claude-skills**](https://github.com/YANZHANLIN/ielts-claude-skills) | `0.2k` | 雅思备考 AI 教练，4 个无状态 Skill 覆盖写作 / 阅读 / 口语训练。 |
| [**zhaihao118/Micro-Drama-Skills**](https://github.com/zhaihao118/Micro-Drama-Skills) | `0.2k` | AI 驱动的短剧全流程自动化：从剧本、角色设计、分镜到视频提交的完整工作流。 |
| [**Fokkyp/claude-skills**](https://github.com/Fokkyp/claude-skills) | `0.2k` | 产品经理实战 Skills 仓库，竞品分析、需求文档等均已在商业环境验证。 |
| [**zouchenzhen/thesis-defense-pptx-skill**](https://github.com/zouchenzhen/thesis-defense-pptx-skill) | `0.2k` | 从论文 PDF / LaTeX 生成可编辑的答辩 PPTX，并保留指定 PPT 模板风格。 |
| [**runapi-ai/cli-skill**](https://github.com/runapi-ai/cli-skill) | `0.1k` | 面向 Claude Code、Codex 等智能体的 RunAPI CLI Skill，可调用图片、视频、音乐 / 音频、语音和 LLM 等模型 API 任务。 |
| [**iamzifei/wechat-article-publisher-skill**](https://github.com/iamzifei/wechat-article-publisher-skill) | `0.1k` | 一键发布文章到微信公众号的 Claude Skill。 |

---

## 🛠️ 工具与基础设施 (Tools & Infrastructure)

用于运行、测试、部署或增强 Claude Skills 的周边工具。

| 项目 | 类型 | ⭐ Stars | 简介 |
| :--- | :--- | ---: | :--- |
| [**vercel-labs/skills**](https://github.com/vercel-labs/skills) | Skill 工具 | `22.5k` | Vercel 官方出品的开放式 agent skills 工具，一行 `npx skills` 即可发现、安装与管理 Skills。 |
| [**diet103/claude-code-infrastructure-showcase**](https://github.com/diet103/claude-code-infrastructure-showcase) | 基建参考 | `9.7k` | 经生产环境验证的 Claude Code 基础设施参考库，为企业级部署提供参考架构。 |
| [**glitternetwork/pinme**](https://github.com/glitternetwork/pinme) | 一键部署 | `3.6k` | 单条命令部署前端应用，原生支持 Claude Code Skills 集成。 |
| [**browserwing/browserwing**](https://github.com/browserwing/browserwing) | 浏览器自动化 | `1.3k` | 把浏览器操作封装成 MCP 命令或 Claude Skill，让智能体直接调用命令控制浏览器，省去高 token 的 LLM 交互。 |
| [**skills-directory/skill-codex**](https://github.com/skills-directory/skill-codex) | 跨智能体 | `1.3k` | 将 prompt 委派给 Codex 执行的 Claude Code skill，实现 Claude 与 Codex 协同。 |
| [**alirezarezvani/claude-code-skill-factory**](https://github.com/alirezarezvani/claude-code-skill-factory) | Skill 工厂 | `0.8k` | 构建与发布 Claude Code Skills 的开源工具包，快速搭建标准化开发流水线。 |
| [**sandiiarov/skill-creator**](https://github.com/sandiiarov/skill-creator) | Skill 生成 | `0.7k` | 把任意 MCP server、OpenAPI 规范或 GraphQL 端点在运行时转成 CLI 的 Skill 生成器。 |
| [**K-Dense-AI/claude-skills-mcp**](https://github.com/K-Dense-AI/claude-skills-mcp) | Skill 检索 | `0.4k` | 用向量检索搜索与调取 Claude Agent Skills 的 MCP server。 |
| [**majiayu000/claude-skill-registry**](https://github.com/majiayu000/claude-skill-registry) | Skill 目录 | `0.4k` | 号称最全的 Claude Code Skills 注册表，配套网页检索站点。 |
| [**instavm/open-skills**](https://github.com/instavm/open-skills) | 跨平台运行器 | `0.4k` | 让 Claude Skills 在本地运行于**任何 LLM** 之上，打破模型限制。 |
| [**smallnest/goskills**](https://github.com/smallnest/goskills) | 跨 LLM 运行器 | `0.2k` | 知名 Go 开发者 smallnest 出品，让 OpenAI 等任意 LLM 也能使用 Claude Skills，并可作为子智能体。 |
| [**wanghuan9/skill-manager**](https://github.com/wanghuan9/skill-manager) (SkillDock) | Skill 管理 | `0.2k` | 安装、查看、更新与同步 AI skills 与 MCP servers 的管理器，支持 Git-aware 更新以跟踪上游变更与本地修改。 |
| [**GBSOSS/mcp-to-skill-converter**](https://github.com/GBSOSS/-mcp-to-skill-converter) | MCP 转换 | `0.2k` | 把任意 MCP server 转换成 Claude Skill，节省约 90% 上下文。 |
| [**huifer/skill-security-scan**](https://github.com/huifer/skill-security-scan) | 安全扫描 | `0.1k` | 安装第三方 Skill 前先做安全审查的命令行工具，检测窃取数据或破坏系统的恶意代码。 |
| [**Xquik-dev/x-twitter-scraper**](https://github.com/Xquik-dev/x-twitter-scraper) | 数据抓取 | `0.1k` | X/Twitter 数据抓取技能，提供 MCP 服务器与 REST API，含 20 个提取工具。 |

---

## 📚 精选资源集合 (Awesome Collections)

社区其他维护者整理的相关 Awesome 列表，便于交叉参考。

| 项目 | ⭐ Stars | 简介 |
| :--- | ---: | :--- |
| [**ComposioHQ/awesome-claude-skills**](https://github.com/ComposioHQ/awesome-claude-skills) | `63.4k` | 目前 Star 最高的 Claude Skills Awesome 列表，由 Composio 团队维护，收录海量 Skills、资源与工具。 |
| [**sickn33/antigravity-awesome-skills**](https://github.com/sickn33/antigravity-awesome-skills) | `39.8k` | 面向 Antigravity 场景的 Skills 精选列表，补充了大量社区贡献的资源与用法。 |
| [**VoltAgent/awesome-agent-skills**](https://github.com/VoltAgent/awesome-agent-skills) | `24.4k` | 另一视角下的 Agent / Claude Skills 资源精选合集，含不同的工具与案例。 |
| [**travisvn/awesome-claude-skills**](https://github.com/travisvn/awesome-claude-skills) | `13.2k` | 精选的 Claude Skills、资源和工具列表，特别关注 **Claude Code** 的工作流集成。 |
| [**BehiSecc/awesome-claude-skills**](https://github.com/BehiSecc/awesome-claude-skills) | `9.4k` | 另一份高 Star 的 Claude Skills 精选列表。 |
| [**davepoon/buildwithclaude**](https://github.com/davepoon/buildwithclaude) | `3.0k` | 一站式中心，集中发现 Skills、Agents、Commands、Hooks、Plugins 与 Marketplace 资源。 |
| [**abubakarsiddik31/claude-skills-collection**](https://github.com/abubakarsiddik31/claude-skills-collection) | `0.7k` | 汇集官方与社区构建的 Claude Skills，便于一站式浏览扩展 Anthropic 能力。 |
| [**karanb192/awesome-claude-skills**](https://github.com/karanb192/awesome-claude-skills) | `0.4k` | 50+ 经验证的 Claude Skills 精选，覆盖 TDD、调试、Git 工作流、文档处理等，社区驱动持续维护。 |

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
