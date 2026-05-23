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

## License

These skill files are provided for evaluation and competition purposes. The Danmo Scholar platform and its core AI generation engine are proprietary.
