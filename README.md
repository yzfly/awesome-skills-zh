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
- [**anthropics/skills**](https://github.com/anthropics/skills)  
  Anthropic 官方维护的 Agent Skills 公共仓库。这是理解标准实现的最佳参考，包含了多个基础且通用的 Skill 范例。

- [**anthropics/claude-cookbooks (skills)**](https://github.com/anthropics/claude-cookbooks/tree/main/skills)  
  Anthropic 官方 Cookbook 中的 Skills 目录，提供了可直接运行的端到端示例与教程，适合上手实践。

#### 🚀 社区框架
- [**obra/superpowers**](https://github.com/obra/superpowers)  
  2026 年最具影响力的社区 Skills 框架之一，已入选官方插件市场。提供了一整套可组合的高级 Skill 与工作流，大幅增强 Claude 的自主能力。

- [**obra/superpowers-marketplace**](https://github.com/obra/superpowers-marketplace)  
  Superpowers 配套的 Skills 市场，便于发现、安装与分享社区贡献的 Skills。

#### 📚 大型 Skills 合集 (高 Star)
- [**alirezarezvani/claude-skills**](https://github.com/alirezarezvani/claude-skills) `⭐ 17.3k`  
  社区最大规模的 Skills 合集之一，收录 **337** 个 Claude Code skills、agent skills 与插件，含 30+ Agents、70+ 自定义命令，开箱即用。

- [**Jeffallan/claude-skills**](https://github.com/Jeffallan/claude-skills) `⭐ 9.7k`  
  面向全栈开发者的 **66** 个专精 Skills，将 Claude Code 打造成你的全栈开发搭档。

- [**mohitagw15856/pm-claude-skills**](https://github.com/mohitagw15856/pm-claude-skills) `⭐ 0.9k`  
  覆盖 17 个职业方向的 **167** 个专业级 Skills，主打产品 / 项目管理与办公场景效率提升。

- [**mrgoonie/claudekit-skills**](https://github.com/mrgoonie/claudekit-skills) `⭐ 2.1k`  
  ClaudeKit.cc 出品的全套高质量 Skills 合集。

- [**coleam00/second-brain-skills**](https://github.com/coleam00/second-brain-skills) `⭐ 0.8k`  
  将 Claude Code 变成「第二大脑」的 Skills 合集，主打知识管理与个人信息检索。

- [**jezweb/claude-skills**](https://github.com/jezweb/claude-skills) `⭐ 0.8k`  
  面向 Claude Code CLI 的全栈开发 Skills，涵盖 Cloudflare、React、Tailwind v4 等技术栈。

- [**simonw/claude-skills**](https://github.com/simonw/claude-skills) `⭐ 0.9k`  
  Simon Willison 整理的 Claude 代码解释器环境中 `/mnt/skills` 目录的真实内容，适合研究官方内置 Skill 的实现。

#### 🔬 垂直领域
- [**claude-scientific-skills**](https://github.com/K-Dense-AI/claude-scientific-skills)  
  包含 **125+** 个现成的科学研究类 Skills。专为科研场景设计，涵盖了文献分析、数据处理等专业领域。

- [**jherrodthomas/automotive-skills-suite**](https://github.com/jherrodthomas/automotive-skills-suite) `⭐ 1.6k`  
  **100+** 个汽车工程领域 Skills，覆盖 ISO 26262 功能安全等专业工程场景。

- [**tradermonty/claude-trading-skills**](https://github.com/tradermonty/claude-trading-skills) `⭐ 1.8k`  
  面向股票投资者与交易员的 Skills，提供市场分析、技术指标等能力。

- [**aaron-he-zhu/seo-geo-claude-skills**](https://github.com/aaron-he-zhu/seo-geo-claude-skills) `⭐ 2.0k`  
  20 个 SEO 与 GEO（生成式引擎优化）Skills，兼容 Claude Code、Cursor、Codex 等 35+ AI 智能体。

- [**nowork-studio/NotFair**](https://github.com/nowork-studio/NotFair) `⭐ 2.8k`  
  开源的营销增长 Skills，覆盖 SEO、GEO、Google Ads、Meta Ads 等投放场景。

- [**dominikmartn/nothing-design-skill**](https://github.com/dominikmartn/nothing-design-skill) `⭐ 2.4k`  
  以 Nothing 设计语言（单色、点阵风）生成 UI 的 Skill。

#### 🛠️ 生态集成
- [**synalinks-skills**](https://github.com/SynaLinks/synalinks-skills)  
  专为 **Synalinks** 生态系统设计的 Claude Skills 集合，展示了 Skills 在特定框架下的应用。

---

## ⭐ 明星单项 Skills (Featured Standalone Skills)

GitHub 上 Star 数最高、最具话题度的单一用途 Skill。它们大多只做一件事，却把这件事做到极致，是学习「一个好 Skill 该长什么样」的绝佳范例。

- [**multica-ai/andrej-karpathy-skills**](https://github.com/multica-ai/andrej-karpathy-skills) `⭐ 168.9k`  
  **目前 GitHub 上 Star 数最高的 Skill 项目**，由华人开发者 **Forrest Chang** 维护。用一个 `CLAUDE.md` 文件提炼 Andrej Karpathy 的编程理念，显著改善 Claude Code 的行为表现。中文翻译版见 [LearnPrompt/andrej-karpathy-skills](https://github.com/LearnPrompt/andrej-karpathy-skills)。

- [**JuliusBrussee/caveman**](https://github.com/JuliusBrussee/caveman) `⭐ 69.3k`  
  > *"why use many token when few token do trick"* —— 通过精简表达大幅削减 token 消耗的 Claude Code skill，是目前 Star 最高的单项 Skill。

- [**OthmanAdi/planning-with-files**](https://github.com/OthmanAdi/planning-with-files) `⭐ 22.8k`  
  实现 Manus 风格「持久化 Markdown 规划」的 Skill，让 Claude 把任务计划落盘成文件，长任务不丢失上下文。

- [**blader/humanizer**](https://github.com/blader/humanizer) `⭐ 22.6k`  
  去除文本中「AI 味」痕迹的 Skill，让 Claude 生成的文字更自然、更像人写的。

- [**nidhinjs/prompt-master**](https://github.com/nidhinjs/prompt-master) `⭐ 8.9k`  
  为任意 AI 工具自动撰写精准提示词的 Skill，零 token 调用外部 API。

- [**SawyerHood/dev-browser**](https://github.com/SawyerHood/dev-browser) `⭐ 6.2k`  
  赋予智能体使用网页浏览器能力的 Skill，让 Claude 能够真正「上网」操作。

- [**zarazhangrui/codebase-to-course**](https://github.com/zarazhangrui/codebase-to-course) `⭐ 4.6k`  
  将任意代码库转化为精美、可交互教程的 Skill，适合做技术布道与上手文档。

- [**virgiliojr94/book-to-skill**](https://github.com/virgiliojr94/book-to-skill) `⭐ 4.4k`  
  把任意技术书籍 PDF 转化为一个可学习、可引用的 Claude Code skill。

- [**lackeyjb/playwright-skill**](https://github.com/lackeyjb/playwright-skill) `⭐ 2.7k`  
  基于 Playwright 的浏览器自动化 Skill，模型可按需自动调用完成网页操作。

- [**dgreenheck/webgpu-claude-skill**](https://github.com/dgreenheck/webgpu-claude-skill) `⭐ 1.0k`  
  用于 Three.js + WebGPU 应用开发的 Skill。

---

## 🔒 安全与逆向 (Security & Reverse Engineering)

面向安全研究、渗透测试、漏洞挖掘与逆向工程的 Skills。请仅在获得授权的合法测试与研究场景下使用。

- [**SimoneAvogadro/android-reverse-engineering-skill**](https://github.com/SimoneAvogadro/android-reverse-engineering-skill) `⭐ 6.0k`  
  辅助 Android App 逆向工程的 Claude Code skill。

- [**trailofbits/skills**](https://github.com/trailofbits/skills) `⭐ 5.6k`  
  知名安全公司 **Trail of Bits** 出品，面向安全研究、漏洞检测的 Skills 合集。

- [**SnailSploit/Claude-Red**](https://github.com/SnailSploit/Claude-Red) `⭐ 2.1k`  
  面向红队 / 攻击性安全的精选 Skill 库。

- [**elementalsouls/Claude-BugHunter**](https://github.com/elementalsouls/Claude-BugHunter) `⭐ 1.7k`  
  用于漏洞挖掘与外部红队作业的 Skill bundle，含 71+ 模块。

- [**elementalsouls/Claude-OSINT**](https://github.com/elementalsouls/Claude-OSINT) `⭐ 1.6k`  
  两个配套的 OSINT 情报搜集 Skill，含 90+ 侦察模块、48 个密钥正则与 80+ 数据源。

- [**BrownFineSecurity/iothackbot**](https://github.com/BrownFineSecurity/iothackbot) `⭐ 0.8k`  
  面向 IoT 渗透测试的 Claude Skills 与定制工具集合。

---

## 🧬 女娲 · 人物思维 Skill 生态 (Nuwa / Persona Skills)

由华人开发者 **花叔 ([@alchaincyf](https://github.com/alchaincyf))** 发起的「人物认知操作系统」Skill 生态，是 2026 年中文社区最具影响力的现象级 Skill 玩法——**蒸馏任何人的思维方式**（心智模型、决策启发式、表达 DNA），让 Claude 以特定人物的思维框架运行。

#### 核心引擎
- [**alchaincyf/nuwa-skill (女娲.skill)**](https://github.com/alchaincyf/nuwa-skill) `⭐ 22.9k`  
  生态本体。「你想蒸馏的下一个员工，何必是同事」——输入任意人物，自动生成一个可运行的人物思维 Skill。下方多数人物 Skill 均由它生成。

- [**alchaincyf/darwin-skill (达尔文.skill)**](https://github.com/alchaincyf/darwin-skill) `⭐ 3.5k`  
  让你的 Skill 无限进化的系统：评估 → 改进 → 测试 → 保留或回滚，受 AutoResearch 启发的自我迭代机制。

#### 人物认知操作系统 (由女娲生成)
- [**alchaincyf/zhangxuefeng-skill (张雪峰.skill)**](https://github.com/alchaincyf/zhangxuefeng-skill) `⭐ 7.6k` — 高考志愿 / 考研 / 职业规划的实战思维框架。

> 此外，作者还用女娲生成了乔布斯、马斯克、芒格、特朗普、Karpathy、纳瓦尔、费曼等多位人物的 `.skill`（均在 1k star 以下），完整清单见下方 awesome 列表与 [作者主页](https://github.com/alchaincyf?tab=repositories)。

#### 生态精选列表
- [**tmstack/awesome-persona-skills**](https://github.com/tmstack/awesome-persona-skills) `⭐ 2.6k` — 同事.skill、老板.skill、前任.skill、永生.skill、女娲.skill…… 人物 Skill 大合集。
- [**Panmax/awesome-nuwa**](https://github.com/Panmax/awesome-nuwa) `⭐ 0.1k` — 用女娲蒸馏的人物思维框架合集。

---

## 🇨🇳 中文社区 Skills (Chinese Community)

由中文社区作者开发、文档以中文为主或专为中文场景优化的 Skills，对国内用户尤为友好。

- [**alchaincyf/huashu-design (花叔设计)**](https://github.com/alchaincyf/huashu-design) `⭐ 16.4k`  
  花叔出品的 HTML-native 设计 Skill for Claude Code，让 Claude 直接产出高质量网页设计。

- [**op7418/Humanizer-zh**](https://github.com/op7418/Humanizer-zh) `⭐ 9.3k`  
  [blader/humanizer](https://github.com/blader/humanizer) 的汉化版本，专门用于消除中文文本中的 AI 生成痕迹。

- [**joeseesun/qiaomu-anything-to-notebooklm**](https://github.com/joeseesun/qiaomu-anything-to-notebooklm) `⭐ 4.9k`  
  面向 NotebookLM 的多源内容处理 Skill，支持微信公众号文章等多种来源的内容采集与整理。

- [**Ceeon/videocut-skills**](https://github.com/Ceeon/videocut-skills) `⭐ 1.8k`  
  用 Claude Code Skills 打造的视频剪辑 Agent。

- [**alchaincyf/huashu-skills (花叔内容创作)**](https://github.com/alchaincyf/huashu-skills) `⭐ 0.9k`  
  花叔的内容创作 Skills 合集，含 AI 审校、选题生成、视频大纲、素材搜索等 11 个实用技能。

- [**huangserva/skill-prompt-generator**](https://github.com/huangserva/skill-prompt-generator) `⭐ 1.4k`  
  基于 Claude Skill 的 AI 人像 Prompt 生成系统，可从特征库智能组合生成高质量人像描述，并具备自动学习与扩展能力。

- [**P4nda0s/reverse-skills**](https://github.com/P4nda0s/reverse-skills) `⭐ 1.2k`  
  逆向工程 Claude Code Skills 插件，中文文档友好。

- [**yzfly/douyin-mcp-server**](https://github.com/yzfly/douyin-mcp-server) `⭐ 1.0k`  
  提取抖音无水印视频链接与视频文案，同时支持 MCP 与 Claude Skill 两种调用方式（本仓库维护者 [@yzfly](https://github.com/yzfly) 出品）。

---

## 🛠️ 工具与基础设施 (Tools & Infrastructure)

用于运行、测试、部署或增强 Claude Skills 的周边工具。

- [**Open Skills**](https://github.com/BandarLabs/open-skills)  
   **跨平台运行器** - 该项目旨在让 Claude Skills 能够本地运行在**任何 LLM** 之上 (OpenSkills: Run Claude Skills Locally using any LLM)，打破了模型的限制。

- [**claude-code-infrastructure-showcase**](https://github.com/diet103/claude-code-infrastructure-showcase)  
   **基建参考** - 一个精选的、经过生产环境测试的 Claude Code 基础设施参考库，为企业级部署提供了宝贵的参考架构。

- [**alirezarezvani/claude-code-skill-factory**](https://github.com/alirezarezvani/claude-code-skill-factory) `⭐ 0.8k`  
   **Skill 工厂** - 用于构建与发布 Claude Code Skills 的开源工具包，帮助你快速搭建标准化的 Skill 开发流水线。

- [**glitternetwork/pinme**](https://github.com/glitternetwork/pinme) `⭐ 3.6k`  
   **一键部署** - 单条命令部署前端应用，原生支持 Claude Code Skills 集成。

- [**skills-directory/skill-codex**](https://github.com/skills-directory/skill-codex) `⭐ 1.3k`  
   **跨智能体协作** - 一个将 prompt 委派给 Codex 执行的 Claude Code skill，实现 Claude 与 Codex 的协同。

- [**Xquik-dev/x-twitter-scraper**](https://github.com/Xquik-dev/x-twitter-scraper) `⭐ 0.1k`  
   **数据抓取** - X/Twitter 数据抓取技能，提供 MCP 服务器与 REST API，含 20 个提取工具（用户资料、推文、粉丝、关注者等）。

---

## 📚 精选资源集合 (Awesome Collections)

社区其他维护者整理的相关 Awesome 列表，便于交叉参考。

- [**awesome-claude-skills (ComposioHQ)**](https://github.com/ComposioHQ/awesome-claude-skills) `⭐ 63.4k`  
  目前 Star 数最高的 Claude Skills Awesome 列表，由 Composio 团队维护，收录了海量定制 Claude 的 Skills、资源与工具。

- [**awesome-claude-skills (TravisVN)**](https://github.com/travisvn/awesome-claude-skills) `⭐ 13.2k`  
  精选的 Claude Skills、资源和工具列表，内容特别关注 **Claude Code** 的工作流集成。

- [**awesome-claude-skills (BehiSecc)**](https://github.com/BehiSecc/awesome-claude-skills) `⭐ 9.4k`  
  另一份高 Star 的 Claude Skills 精选列表。

- [**davepoon/buildwithclaude**](https://github.com/davepoon/buildwithclaude) `⭐ 3.0k`  
  一站式中心，集中发现 Claude Skills、Agents、Commands、Hooks、Plugins 与 Marketplace 资源。

- [**abubakarsiddik31/claude-skills-collection**](https://github.com/abubakarsiddik31/claude-skills-collection) `⭐ 0.7k`  
  汇集官方与社区构建的 Claude Skills，便于一站式浏览扩展 Anthropic 能力。
  
- [**awesome-claude-skills (VoltAgent)**](https://github.com/VoltAgent/awesome-claude-skills)  
  另一个视角下的 Claude Skills 和资源精选合集，包含了不同的工具和案例。

- [**antigravity-awesome-skills (sickn33)**](https://github.com/sickn33/antigravity-awesome-skills)  
  面向 Antigravity 场景的 Skills 精选列表，补充了更多社区贡献的资源与用法。

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