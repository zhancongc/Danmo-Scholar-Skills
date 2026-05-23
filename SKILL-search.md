# Skill: Academic Literature Search

Search academic papers by research topic across 200M+ papers from OpenAlex and Semantic Scholar databases.

## Authentication

This skill requires a Bearer token. Follow this flow to obtain one:

### Step 1: Ask the user for their email
Say: "Please provide your email address to log in to Danmo Scholar."

### Step 2: Send verification code
```
POST https://scholar.danmo.tech/api/auth/send-code
Content-Type: application/json

{
  "email": "user@example.com",
  "purpose": "login"
}
```

Response:
```json
{ "success": true, "message": "code_sent_to_u***@example.com" }
```

### Step 3: Ask for verification code
Say: "A 6-digit verification code has been sent to your email. Please enter it here."

### Step 4: Login
```
POST https://scholar.danmo.tech/api/auth/login-with-code
Content-Type: application/json

{
  "email": "user@example.com",
  "code": "123456"
}
```

Response:
```json
{
  "success": true,
  "token": "eyJhbGciOiJIUzI1NiIs...",
  "user": { "id": 1, "email": "user@example.com", "nickname": "..." }
}
```

**Save the `token` value** — it will be used in all subsequent requests as `Authorization: Bearer <token>`.

> If the user already has a token, ask them to provide it directly and skip Steps 1-4.

---

## Skill: Search Papers

### When to use
When the user wants to find academic papers on a specific research topic.

### Instructions

1. Ask the user for their research topic (required) and preferences:
   - Target paper count (default: 50, range: 10-100)
   - Search year range (default: 10 years, range: 5-30)

2. Call the search API:

```
POST https://scholar.danmo.tech/api/search-papers-only
Content-Type: application/json
Authorization: Bearer <token>

{
  "topic": "photocatalytic water splitting",
  "target_count": 50,
  "search_years": 10
}
```

3. The API uses an AI agent (PaperSearchAgent) to:
   - Expand the topic into multiple search queries
   - Search OpenAlex (primary) and Semantic Scholar (fallback)
   - Deduplicate and rank results by relevance

4. Response format:
```json
{
  "success": true,
  "message": "literature_search_completed",
  "data": {
    "papers": [
      {
        "title": "Paper Title",
        "authors": ["Author A", "Author B"],
        "year": 2024,
        "abstract": "...",
        "cited_by_count": 42,
        "doi": "10.1234/...",
        "venue": "Nature",
        "source": ["openalex", "semantic_scholar"]
      }
    ],
    "total_count": 47,
    "search_queries": ["photocatalytic water splitting", "TiO2 hydrogen production"]
  }
}
```

5. Present the results to the user:
   - Show total papers found
   - List top papers with title, authors, year, citations, and venue
   - Highlight highly cited papers

6. After showing results, suggest next steps:
   - "Would you like me to generate a comparison matrix for these papers?"
   - "Would you like me to generate a comprehensive literature review on this topic?"

### Error Handling

| Status | Detail | Action |
|--------|--------|--------|
| 401 | `login_required` | Re-authenticate (go back to Step 1) |
| 429 | `search_limit_reached` | Daily free search limit reached. Tell user to try tomorrow or purchase credits. |
| 500 | Server error | Retry once, then inform user |

### Cost
- Free tier: 30 searches per day
- Can exchange credits for additional searches (1 credit = 30 searches)

### Examples

**User**: "Help me find papers about brain-computer interfaces for stroke rehabilitation"

**AI**: Calls `POST /api/search-papers-only` with `{"topic": "brain-computer interfaces for stroke rehabilitation", "target_count": 50, "search_years": 10}`, then presents the paper list.
