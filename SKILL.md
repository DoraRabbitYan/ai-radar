---
name: ai-radar
description: AI 雷达——多源 AI 热点采集与验证工具。从 Twitter/X、Product Hunt、Reddit、Hacker News、博客等聚合扫描 AI 热点，经原文抓取验证后输出结构化日报。当用户说"开始今日选题"、"采集热点"、"看看今天有什么新闻"、"今日AI热点"时触发。聚焦领域：Vibe Coding、Claude Skill、AI 知识管理、AI 模型更新、AI 新产品、海外热点。
---

# AI Radar - AI 热点雷达

## 采集流程（两阶段）

**阶段一 — 搜索发现：**
1. 使用 `web_search` 对各平台并行搜索
2. 从搜索结果中提取有价值的文章/帖子 URL

**阶段二 — 原文抓取（强制）：**
3. 对每条有价值结果，使用 `web_fetch` 抓取原文内容
4. 基于原文（而非搜索摘要）提炼要点
5. 分类整理，输出结构化列表

> ⚠️ **核心原则：不得仅凭搜索摘要输出。每条热点必须经过 web_fetch 抓取原文后，再用原文提炼。**

## 数据源分类

### 一、AI博主/KOL（实践玩法、观点分享）

**Twitter/X 重点账号**：
- @AnthropicAI - Anthropic官方
- @OpenAI - OpenAI官方
- @kaborsk1 - Boris Cherny（Claude Code创作者）
- @swyx - AI工程师，Latent Space播客
- @simonw - Simon Willison，AI工具实践
- @levelsio - Pieter Levels，AI独立开发
- @emaborski - Eugene Borukhovich，AI健康
- @maborosi - AI自动化工作流

**搜索关键词**：
```
"Claude Code" tips OR tricks OR workflow 2026
"Cursor" vibe coding best practices 2026
AI agent automation n8n 2026
Claude MCP server 2026
```

### 二、创业公司/产品发布

**Product Hunt**：
- 搜索：AI、Developer Tools、Productivity分类
- 当日/当周上榜产品
- 提取：产品名 + 描述 + upvotes + 产品页链接
- **必须 web_fetch 产品页获取详细描述**

**Hacker News**：
- Show HN中的AI项目
- Launch HN中的AI创业公司
- 提取：标题 + 讨论链接 + 评论数

### 三、AI研究员/学术动态

**关注来源**：
- arXiv热门AI论文（被引用/讨论多的）
- Google DeepMind博客
- Meta AI博客
- Microsoft Research

**搜索关键词**：
```
site:arxiv.org AI agent OR LLM May 2026
site:deepmind.google AI research May 2026
site:ai.meta.com new paper May 2026
```

### 四、模型厂商官方动态

| 厂商 | 搜索关键词 |
|------|------------|
| Anthropic | site:anthropic.com OR "Claude" new feature May 2026 |
| OpenAI | site:openai.com OR "ChatGPT" update May 2026 |
| Google | "Gemini" update OR site:blog.google AI May 2026 |
| xAI | "Grok" update May 2026 |
| Mistral | site:mistral.ai May 2026 |

### 五、技术社区讨论

**Reddit子版**：
- r/ClaudeAI
- r/ChatGPT
- r/LocalLLaMA
- r/artificial
- r/MachineLearning

**Hacker News**：
- AI相关热门讨论
- Show HN项目

## 聚焦领域（优先级排序）

1. **Vibe Coding** - 自然语言编程、Cursor、Claude Code
2. **Claude生态** - Claude Skill、MCP Server、Claude Code技巧
3. **AI Agent** - 自动化工作流、n8n、Make
4. **AI知识管理** - 第二大脑、PKM、Obsidian+AI
5. **模型更新** - GPT、Claude、Gemini版本发布
6. **AI新产品** - Product Hunt上榜、独立开发者作品
7. **海外热点** - 行业大事件、收购、融资

## 输出格式

```markdown
## 今日AI热点 - MMDD

---

### 🧑‍💻 AI博主实践分享

1. **[标题]**
   - 来源：[原文URL](https://...)
   - 作者：@用户名
   - 原文摘要：(基于 web_fetch 抓取的实际内容提炼，3-5句话)
   - 热度：❤️ likes | 🔁 retweets

---

### 🚀 创业公司/新产品

1. **[产品名]** - 一句话描述
   - 链接：[Product Hunt页面](https://producthunt.com/posts/xxx)
   - 原文摘要：(基于 web_fetch 产品页抓取的实际内容)
   - 热度：⬆️ N upvotes
   - 分类：AI / DevTools / Productivity

---

### 🔬 AI研究/学术动态

1. **[论文/博客标题]**
   - 来源：DeepMind / Meta AI / arXiv
   - 原文：[URL](https://...)
   - 原文摘要：(基于 web_fetch 抓取的实际内容提炼核心发现，3-5句话)

---

### 🏢 模型厂商动态

1. **[更新/发布内容]**
   - 厂商：Anthropic / OpenAI / Google
   - 原文：[官方博客链接](https://...)
   - 原文摘要：(基于 web_fetch 抓取的实际内容提炼关键变化，3-5句话)

---

### 💬 社区热议

1. **[讨论标题]**
   - 来源：r/ClaudeAI / Hacker News
   - 链接：[帖子URL](https://...)
   - 原文摘要：(基于 web_fetch 抓取的实际讨论内容提炼)
   - 热度：⬆️ upvotes | 💬 comments
```

## 采集执行步骤

### 阶段一：并行搜索（web_search）

同时发起以下搜索，每类 1 次，返回中挑选最有价值的 2-3 条：

```
1. AI博主实践：
   "Claude Code" OR "Cursor" tips tricks May 2026

2. Product Hunt AI产品：
   Product Hunt AI tools top products May 2026

3. 模型厂商更新：
   Anthropic Claude OR OpenAI ChatGPT new feature update May 2026

4. AI Agent工作流：
   AI agent automation workflow n8n vibe coding 2026

5. 社区讨论：
   Reddit ClaudeAI OR ChatGPT hot discussion May 2026

6. 研究动态：
   AI research breakthrough May 2026 DeepMind OR Google OR Meta
```

### 阶段二：原文抓取（web_fetch）— 强制执行

从阶段一的搜索结果中，**每个分类挑 2-3 条最有价值的结果**，用 `web_fetch` 抓取原文：

- 如果搜索结果自带 URL → 直接 web_fetch 该 URL
- 如果搜索结果无明确 URL → 根据标题/来源构造搜索，找到原文再 fetch
- 每个 fetch 设置 `maxChars=8000`，确保拿到足够内容
- 基于 fetch 到的实际内容提炼「原文摘要」，写进输出

> **禁止行为：** 不得跳过 web_fetch 直接用搜索摘要拼凑输出。搜索摘要仅用于「初筛判断这条值不值得抓取」。

## 筛选标准：什么算「最有价值」

搜索结果出来后，按以下 5 个维度打分（每条 0-2 分，满分 10 分），取分数最高的 2-3 条进入 web_fetch：

| 维度 | 0 分 | 1 分 | 2 分 |
|------|------|------|------|
| **🎯 领域匹配** | 与 7 大聚焦领域无关 | 泛 AI 相关 | 精准命中聚焦领域 |
| **🛠️ 实操价值** | 纯观点/碎碎念 | 有方法论但无步骤 | 有具体步骤/命令/案例 |
| **📅 时效性** | 超过 2 周 | 本周内 | 24 小时内 |
| **👤 来源权威** | 匿名/不可考 | 一般技术博客 | 知名 KOL / 官方 / 顶刊 |
| **🔥 热度信号** | 无互动数据 | 有少量互动 | 高赞/高评论/高引用 |

**筛选流程：**
```
搜索结果 → 逐条打分 → 排序 → 取 Top 2-3 → web_fetch
```

**特殊规则：**
- 同一个人/同一个产品的多条结果，只取分数最高的一条
- 刚过 7 分的才进入 web_fetch（低于 7 分直接丢弃）
- Product Hunt 产品额外 +1 分（因为是新事物，实操价值天然高）

## 注意事项

- **两阶段缺一不可**：搜索 → 打分筛选 → fetch 原文 → 提炼 → 输出
- **原文链接必须可点击**：每条热点都要有通过 fetch 验证过的具体 URL
- 不采集纯学术论文（除非引发广泛讨论）
- 不采集重复/相似内容，合并同类
- 标注内容语言（中/英）
- **每个分类最终输出 2-5 条**，保证信息密度而非数量
- 输出报告末尾附打分摘要表，让用户看到筛选逻辑
