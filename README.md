# 澹墨学术 Danmo Scholar Skills

AI 驱动的学术文献研究技能包，可安装到 OpenClaw、Cursor、Windsurf 等 AI 智能体中使用。

澹墨学术是一个已上线的 AI 文献综述生成平台 ([scholar.danmo.tech](https://scholar.danmo.tech))，本仓库提供三个核心技能，评委可直接在 AI 智能体中安装体验。

## 技能列表

| 技能 | 文件 | 说明 | 费用 |
|------|------|------|------|
| 文献搜索 | [SKILL-search.md](SKILL-search.md) | 基于 OpenAlex + Semantic Scholar 搜索学术论文 | 每天免费 5 次 |
| 对比矩阵 | [SKILL-matrix.md](SKILL-matrix.md) | 自动生成结构化文献对比表格 | 1 积分 |
| 文献综述 | [SKILL-review.md](SKILL-review.md) | AI 生成完整学术文献综述（含引用） | 2 积分 |

---

## 评委试用指南

### 第一步：安装技能

**方式 A — OpenClaw：**

将仓库地址添加到技能配置：
```
https://github.com/zhancongc/Danmo-Scholar-Skills
```

**方式 B — Cursor / Windsurf / Cline：**

将 [SKILL-search.md](SKILL-search.md)、[SKILL-matrix.md](SKILL-matrix.md)、[SKILL-review.md](SKILL-review.md) 的内容直接粘贴到 AI 对话中。

**方式 C — Claude Code：**

```bash
git clone https://github.com/zhancongc/Danmo-Scholar-Skills.git
```
然后告诉 AI：`请阅读 SKILL-search.md 并执行`

### 第二步：配置 Token

已为评委预配置演示 Token（有效期 3 天，已充值 50 积分）：

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI1OSIsImV4cCI6MTc3OTgwNDA5NH0.8L4WYYKtOCQzh99fjp1uIt_xUpLQFwN5IldNf2ZdChw
```

告诉 AI 智能体：`我的 Danmo Scholar token 是 eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI1OSIsImV4cCI6MTc3OTgwNDA5NH0.8L4WYYKtOCQzh99fjp1uIt_xUpLQFwN5IldNf2ZdChw`

**验证 Token：**

```bash
curl -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiI1OSIsImV4cCI6MTc3OTgwNDA5NH0.8L4WYYKtOCQzh99fjp1uIt_xUpLQFwN5IldNf2ZdChw" https://scholar.danmo.tech/api/skill/check-token
```

### 第三步：试用示例

#### 示例 1：搜索文献

> 请帮我搜索关于「光催化分解水制氢」的学术论文，搜索 50 篇。

返回来自 OpenAlex 和 Semantic Scholar 的论文列表（标题、作者、年份、被引次数、摘要）。

#### 示例 2：生成对比矩阵

> 请为「Transformer 在计算机视觉中的应用」生成一个文献对比矩阵。

提交任务后约 1-3 分钟返回结构化对比表格。消耗 **1 积分**。

#### 示例 3：生成文献综述

> 请为我生成一篇关于「脑机接口在卒中运动康复中的研究进展」的文献综述，包含 50 篇参考文献。

3 阶段 8 步流程（搜索文献 → 生成综述 → 引用校验），约 3-8 分钟返回完整综述。消耗 **2 积分**。

### 更多推荐主题

| 领域 | 主题 |
|------|------|
| 材料科学 | 钙钛矿太阳能电池的稳定性研究进展 |
| 计算机科学 | 大语言模型在代码生成中的应用 |
| 医学 | 免疫检查点抑制剂在非小细胞肺癌中的研究进展 |
| 环境科学 | 微塑料在水体中的分布与生态风险 |
| 管理学 | 数字化转型对企业创新绩效的影响 |

### 积分消耗

| 功能 | 消耗 | 说明 |
|------|------|------|
| 文献搜索 | 0 积分 | 每天免费 5 次 |
| 对比矩阵 | 1 积分 | 自动扣费 |
| 文献综述 | 2 积分 | 自动扣费 |

演示账号已充值 **50 积分**，足够生成 25 篇综述或 50 个对比矩阵。

---

## 技术架构

```
用户 → AI 智能体 (OpenClaw/Cursor/Windsurf) → SKILL.md → 澹墨学术 API
                                                         ↓
                                                 scholar.danmo.tech
                                                         ↓
                                             OpenAlex + Semantic Scholar
                                             + DeepSeek AI Generation
```

所有计算（文献检索、AI 生成、引用校验）均在澹墨学术服务端完成，技能文件仅包含 API 调用指令。

## 在线体验

访问 [scholar.danmo.tech](https://scholar.danmo.tech) 体验完整 Web 版。

用户安装指南详见 [README-user.md](README-user.md)。
