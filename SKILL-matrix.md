# Skill: Literature Comparison Matrix

Generate a structured comparison table analyzing key dimensions across top papers for a research topic.

## Authentication

This skill requires a Bearer token. Follow the authentication flow described in [SKILL-search.md](SKILL-search.md#authentication), or reuse an existing token.

> If the user already has a token (e.g., from a previous search), reuse it directly.

---

## Skill: Generate Comparison Matrix

### When to use
When the user wants to compare academic papers on a topic in a structured table format.

### Instructions

1. Ask the user for their research topic (required). Optional:
   - Language: `zh` (Chinese, default) or `en` (English)
   - If they previously searched papers, ask if they want to reuse those results

2. Submit the comparison matrix task:

```
POST https://scholar.danmo.tech/api/generate-comparison-matrix
Content-Type: application/json
Authorization: Bearer <token>

{
  "topic": "photocatalytic water splitting",
  "language": "zh",
  "reuse_task_id": ""
}
```

- If the user previously completed a search, set `reuse_task_id` to the search task ID to skip re-searching.

3. Response (immediate):
```json
{
  "success": true,
  "message": "task_submitted",
  "data": {
    "task_id": "a1b2c3d4"
  }
}
```

4. **Poll for results** — The matrix generation is asynchronous. Poll every 5 seconds:

```
GET https://scholar.danmo.tech/api/comparison-matrix/{task_id}
Authorization: Bearer <token>
```

5. Polling responses:

**In progress:**
```json
{
  "success": true,
  "data": {
    "task_id": "a1b2c3d4",
    "status": "processing",
    "current_stage": "generating_comparison_matrix",
    "topic": "photocatalytic water splitting"
  }
}
```

**Completed:**
```json
{
  "success": true,
  "data": {
    "task_id": "a1b2c3d4",
    "status": "completed",
    "topic": "photocatalytic water splitting",
    "comparison_matrix": "# Comparison Matrix\n\n| Dimension | Paper 1 | Paper 2 | ... |",
    "papers": [...],
    "statistics": {
      "total_papers": 20,
      "categories": ["Materials Science", "Chemistry"]
    }
  }
}
```

6. Present the comparison matrix to the user:
   - Render the markdown table
   - Summarize key findings
   - Show statistics (total papers, research categories)

7. Suggest next step: "Would you like me to generate a comprehensive literature review on this topic?"

### Polling Logic

```
Submit task → Get task_id
Loop every 5 seconds:
  GET /api/comparison-matrix/{task_id}
  If status == "completed" → Show result, stop polling
  If status == "failed" → Show error, stop polling
  If status == "processing" → Continue polling
  After 5 minutes → Timeout, inform user
```

### Error Handling

| Status | Detail | Action |
|--------|--------|--------|
| 401 | `login_required` | Re-authenticate |
| 400 | `credit_insufficient` | Insufficient credits. Tell user about pricing. |
| 404 | Task not found | Task may have expired. Submit a new one. |

### Cost
- 1 credit per generation (users with credits are auto-deducted)
- Free tier: limited daily free quota available

### Typical Duration
- 1-3 minutes (includes paper search + matrix generation)

### Examples

**User**: "Compare the top papers on transformer architectures in computer vision"

**AI**: Submits comparison matrix task, polls until complete, presents the structured table comparing papers across dimensions like architecture, dataset, accuracy, and computational cost.
