# Danmo Scholar Skills

AI-powered academic literature research skills for intelligent agents.

Danmo Scholar (澹墨学术) is an AI-driven literature review generation platform. This repository provides three skills that can be installed in AI agents (OpenClaw, Windsurf, Cursor, Cline, etc.) to enable academic research capabilities.

## Skills

| Skill | File | Description | Cost |
|-------|------|-------------|------|
| Literature Search | [SKILL-search.md](SKILL-search.md) | Search academic papers by topic using OpenAlex + Semantic Scholar | Free daily quota |
| Comparison Matrix | [SKILL-matrix.md](SKILL-matrix.md) | Generate structured comparison tables from literature | 1 credit |
| Literature Review | [SKILL-review.md](SKILL-review.md) | Generate comprehensive AI-powered literature reviews | 2 credits |

## Installation

### Option 1: OpenClaw

OpenClaw can directly read skill files from a GitHub repository. Add this repo URL in your OpenClaw skill settings:

```
https://github.com/zhancongc/Danmo-Scholar-Skills
```

OpenClaw will automatically load all `SKILL-*.md` files. You can then invoke the skills by name in conversations.

### Option 2: Windsurf / Cursor / Cline

Paste the content of any `SKILL-*.md` file directly into your AI conversation. For example:

```
请阅读以下技能定义并执行：

[粘贴 SKILL-search.md 的全部内容]
```

The AI assistant will read the skill instructions and execute them, calling the Danmo Scholar API on your behalf.

### Option 3: Any AI Agent with File Reading

Clone this repository and instruct your AI agent to read the skill files:

```bash
git clone https://github.com/zhancongc/Danmo-Scholar-Skills.git
```

Then tell your agent:

```
Read the file SKILL-search.md (or SKILL-matrix.md / SKILL-review.md) and follow the instructions.
```

### Installing All Skills at Once

To enable all three skills, paste all three files into your agent's context, or tell the agent:

```
Read all SKILL-*.md files from this repository. I want to use all three skills:
1. Literature Search
2. Comparison Matrix
3. Literature Review
```

## Quick Start

### 1. Authenticate

All skills require a Bearer token from Danmo Scholar. The skill will guide users through email verification login:

```
User provides email → Skill sends verification code → User enters code → Skill obtains token
```

If you already have a token, provide it directly to skip login.

### 2. Usage

Once authenticated, simply tell the AI agent your research topic, and it will:

1. **Search** — Find relevant academic papers across 200M+ papers from OpenAlex and Semantic Scholar
2. **Compare** — Generate structured comparison matrices analyzing key dimensions across top papers
3. **Review** — Produce comprehensive literature reviews with proper citations (IEEE/APA/MLA/GB-T-7714)

## Architecture

```
User → AI Agent (OpenClaw/Cursor/Windsurf) → SKILL.md → Danmo Scholar API
                                                    ↓
                                            scholar.danmo.tech
                                                    ↓
                                        OpenAlex + Semantic Scholar
                                        + DeepSeek AI Generation
```

All heavy computation (paper search, AI generation, citation validation) runs on the Danmo Scholar server. The skill files are lightweight instructions that tell the AI agent how to call the API.

## API Endpoints

| Endpoint | Method | Auth | Description |
|----------|--------|------|-------------|
| `/api/skill/check-token` | GET | Bearer | Verify token validity and check credit balance |
| `/api/auth/send-code` | POST | None | Send email verification code |
| `/api/auth/login-with-code` | POST | None | Login and get token |
| `/api/search-papers-only` | POST | Bearer | Search academic papers |
| `/api/generate-comparison-matrix` | POST | Bearer | Submit comparison matrix task |
| `/api/comparison-matrix/{task_id}` | GET | Bearer | Poll comparison matrix result |
| `/api/smart-generate` | POST | Bearer | Submit literature review task |
| `/api/tasks/{task_id}` | GET | Bearer | Poll task status and result |

**Base URL**: `https://scholar.danmo.tech`

## Credits

- New users get free daily quota for searching and limited free generations
- Paid credits: CNY 9.9 / 19.8 / 49.8 (Chinese) or USD 9.99 / 24.99 / 49.99 (International)
- Literature search: free daily quota (5 searches/day)
- Comparison matrix: 1 credit per generation
- Literature review: 2 credits per generation

## Try Online

Visit [scholar.danmo.tech](https://scholar.danmo.tech) to try the full web experience.

---

## 评委试用指南

欢迎使用澹墨学术（Danmo Scholar）AI 文献研究技能包。本指南帮助您快速体验三个核心功能。

### 第一步：安装技能

**方式 A — 在 OpenClaw 中安装：**

将本仓库地址添加到 OpenClaw 的技能配置中：
```
https://github.com/zhancongc/Danmo-Scholar-Skills
```

**方式 B — 在 Cursor / Windsurf / Cline 中使用：**

将 [SKILL-search.md](SKILL-search.md)、[SKILL-matrix.md](SKILL-matrix.md)、[SKILL-review.md](SKILL-review.md) 的内容直接粘贴到 AI 对话中，AI 会自动读取并执行。

**方式 C — Claude Code 中使用：**

```bash
git clone https://github.com/zhancongc/Danmo-Scholar-Skills.git
```

然后告诉 AI：`请阅读 SKILL-search.md 并执行`

### 第二步：配置 Token

为方便评委试用，我们提供了一个预配置的演示 Token（有效期 3 天，已充值 50 积分）：

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI1OSIsImV4cCI6MTc3OTgwNDA5NH0.8L4WYYKtOCQzh99fjp1uIt_xUpLQFwN5IldNf2ZdChw
```

告诉 AI 智能体：`我的 Danmo Scholar token 是 eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI1OSIsImV4cCI6MTc3OTgwNDA5NH0.8L4WYYKtOCQzh99fjp1uIt_xUpLQFwN5IldNf2ZdChw`

**验证 Token 是否有效：**

```bash
curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI1OSIsImV4cCI6MTc3OTgwNDA5NH0.8L4WYYKtOCQzh99fjp1uIt_xUpLQFwN5IldNf2ZdChw" https://scholar.danmo.tech/api/skill/check-token
```

正常返回：
```json
{
  "success": true,
  "data": {
    "user_id": 59,
    "nickname": "比赛评审",
    "free_credits": 50,
    "paid_credits": 0,
    "total_credits": 50
  }
}
```

### 第三步：试用示例

以下提供三个示例主题，可直接复制给 AI 智能体使用。

#### 示例 1：搜索文献

> 请帮我搜索关于「光催化分解水制氢」的学术论文，搜索 50 篇。

AI 会调用 `POST /api/search-papers-only`，返回来自 OpenAlex 和 Semantic Scholar 的相关论文列表（标题、作者、年份、被引次数、摘要）。

#### 示例 2：生成对比矩阵

> 请为「Transformer 在计算机视觉中的应用」生成一个文献对比矩阵。

AI 会：
1. 提交任务 `POST /api/generate-comparison-matrix`
2. 每隔 5 秒轮询 `GET /api/comparison-matrix/{task_id}`
3. 约 1-3 分钟后返回结构化的对比表格

消耗 **1 积分**。

#### 示例 3：生成文献综述

> 请为我生成一篇关于「脑机接口在卒中运动康复中的研究进展」的文献综述，包含 50 篇参考文献。

AI 会：
1. 提交任务 `POST /api/smart-generate`
2. 每隔 10 秒轮询 `GET /api/tasks/{task_id}`，展示进度（搜索文献 → 生成综述 → 引用校验）
3. 约 3-8 分钟后返回完整的综述（含摘要、关键词、正文、参考文献）

消耗 **2 积分**。

### 更多推荐主题

| 领域 | 主题 |
|------|------|
| 材料科学 | 钙钛矿太阳能电池的稳定性研究进展 |
| 计算机科学 | 大语言模型在代码生成中的应用 |
| 医学 | 免疫检查点抑制剂在非小细胞肺癌中的研究进展 |
| 环境科学 | 微塑料在水体中的分布与生态风险 |
| 管理学 | 数字化转型对企业创新绩效的影响 |

### 积分消耗说明

| 功能 | 消耗 | 说明 |
|------|------|------|
| 文献搜索 | 0 积分 | 每天免费 5 次 |
| 对比矩阵 | 1 积分 | 自动扣费 |
| 文献综述 | 2 积分 | 自动扣费 |

演示账号已充值 **50 积分**，足够生成 25 篇综述或 50 个对比矩阵。

## License

These skill files are provided for evaluation and competition purposes. The Danmo Scholar platform and its core AI generation engine are proprietary.
