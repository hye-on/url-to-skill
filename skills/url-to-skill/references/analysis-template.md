# Service Analysis Template

Organize all research findings into this structure. This document becomes the blueprint for skill design.

## 1. Basic Information

- **Product name**: 
- **URL**: 
- **Tagline**: 
- **One-line description**: 
- **Category**: (no-code, SaaS, productivity, marketing, dev-tools, education, finance, etc.)
- **Target audience**: 
- **Pricing model**: (free, freemium, paid, usage-based, etc.)
- **Tech stack signals**: (visible frameworks, APIs, integrations)

## 2. Core Features

| # | Feature | Description | User Input | Output | Replicable by Claude? | Difficulty |
|---|---------|-------------|-----------|--------|----------------------|------------|
| 1 |         |             |           |        |                      |            |
| 2 |         |             |           |        |                      |            |

## 3. UX Flow

Map the complete user journey step-by-step:

```
Step 1: [User action] → [System response]
Step 2: [User action] → [System response]
...
Step N: [Final result delivered]
```

## 4. Business Logic

### Scoring / Evaluation Model (if applicable)
- Evaluation dimensions:
- Criteria per dimension:
- Scoring formula:
- Grade/tier thresholds:

### Algorithm / Logic (if applicable)
- Data processing approach:
- Recommendation/matching logic:
- Classification criteria:

## 5. UI Pattern

- **Service type**: (quiz / calculator / dashboard / form-wizard / report / editor / chatbot / other)
- **Key UI elements**: (progress bar, charts, cards, tables, radar chart, etc.)
- **Interaction pattern**: (step-by-step, real-time, drag-and-drop, conversational, etc.)
- **Result presentation**: (numeric score, grade, chart, report, downloadable file, etc.)

## 6. Community & Social Signals

### X / Twitter
- Key reactions:
- Common praise:
- Common complaints:

### Reddit
- Relevant threads:
- User sentiment:
- Use cases mentioned:

### Hacker News
- Discussion highlights:
- Technical perspectives:

### Product Hunt
- Upvote count:
- Comment themes:
- Maker responses:

### Reviews / Comparisons
- G2, Capterra, or similar:
- Blog reviews:

## 7. Competitive Landscape

| Competitor | Differentiator | What to learn |
|-----------|----------------|---------------|
|           |                |               |

## 8. Replicability Assessment

### ✅ Fully replicable by Claude
- (list features)

### ⚠️ Partially replicable (needs adaptation)
- (feature → adaptation strategy)

### ❌ Cannot replicate (be honest with user)
- (feature → reason → what to tell user)
- Examples of hard-to-replicate things:
  - Real-time data feeds (stock prices, live analytics)
  - Third-party API integrations requiring auth tokens
  - Multi-user collaboration features
  - Persistent database with user accounts
  - Native mobile app features (push notifications, camera)
  - Payment processing

## 9. Build Plan

### Deliverables
- [ ] SKILL.md (conversational skill)
- [ ] HTML/React artifact (interactive tool)
- [ ] Localhost dashboard
- [ ] Python scripts
- [ ] Reference documents
- [ ] Scoring model

### Recommended tools/MCPs
- (list relevant tools found in environment)

### Estimated complexity
- Simple (< 30 min): single SKILL.md
- Medium (30-60 min): SKILL.md + artifact
- Complex (> 60 min): full package with scripts + dashboard
