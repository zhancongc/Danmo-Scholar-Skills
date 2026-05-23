# Danmo Scholar Skills

AI-powered academic literature research skills for intelligent agents.

Danmo Scholar (澹墨学术) is an AI-driven literature review generation platform. This repository provides three skills that can be installed in AI agents (OpenClaw, Windsurf, Cursor, Cline, etc.) to enable academic research capabilities.

## Skills

| Skill | File | Description | Cost |
|-------|------|-------------|------|
| Literature Search | [SKILL-search.md](SKILL-search.md) | Search academic papers by topic using OpenAlex + Semantic Scholar | Free daily quota |
| Comparison Matrix | [SKILL-matrix.md](SKILL-matrix.md) | Generate structured comparison tables from literature | 1 credit |
| Literature Review | [SKILL-review.md](SKILL-review.md) | Generate comprehensive AI-powered literature reviews | 2 credits |

## Quick Start

### 1. Install Skills

Copy any `SKILL-*.md` file into your AI agent's skill directory, or paste the content directly into a conversation with an AI coding assistant (Cursor, Windsurf, Cline, etc.).

### 2. Authentication

All skills require a Bearer token from Danmo Scholar. The skill will guide users through email verification login:

```
User provides email → Skill sends verification code → User enters code → Skill obtains token
```

If you already have a token, provide it directly to skip login.

### 3. Usage

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
- Literature search: free daily quota (30 searches/day)
- Comparison matrix: 1 credit per generation
- Literature review: 2 credits per generation

## Try Online

Visit [scholar.danmo.tech](https://scholar.danmo.tech) to try the full web experience.

## License

These skill files are provided for evaluation and competition purposes. The Danmo Scholar platform and its core AI generation engine are proprietary.
