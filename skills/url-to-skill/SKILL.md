---
name: url-to-skill
description: "Clones any web product or service into a fully functional Claude skill from just a URL. Deep-researches the product via web search, community reactions (X/Twitter, Reddit, Product Hunt, Hacker News), and live page analysis using Playwright or Chrome MCP when available. Generates a production-ready skill with interactive React/HTML artifacts and scoring models — no backend or server needed. Always use this skill when the user shares a URL and wants to replicate its functionality, says 'clone this service', 'make a skill from this URL', 'copy this product as a skill', 'turn this into a skill', 'build me something like this', or references a Product Hunt / SaaS product they want to reproduce."
---

# URL → Skill Converter

Clone any web product into a production-ready Claude skill from a single URL.

## Pipeline Overview

```
Phase 1         Phase 2          Phase 3        Phase 4          Phase 5
RESEARCH   ───▶ CONSULT    ───▶  DESIGN   ───▶  BUILD      ───▶  VERIFY & DELIVER
Deep Intel      User Needs       Architecture   Generation        QA + Handoff

◆ Quality gate between every phase
◆ Failure recovery at every step
◆ Context carries forward — no phase starts cold
```

This skill turns any publicly accessible web service into a Claude skill that replicates its core value. The output quality should match or exceed the original — not a rough approximation, but a thoughtful reconstruction that understands *why* the product works, not just *what* it does.

---

## Phase 1: Deep Research

The research phase is the foundation of everything. Cutting corners here means building a shallow clone. Go deep.

### 1-1. Tool Selection (Cascading Fallback)

Before starting research, check what tools are available and pick the best option. Document which tier you're using so the user knows the depth of analysis.

| Priority | Tool | Detection | Capabilities |
|----------|------|-----------|-------------|
| Tier 1 | `pw:playwright-pro` | Check available skills | Full browser automation, screenshots, JS rendering, SPA support, form interaction |
| Tier 2 | `mcp__Claude_in_Chrome__*` | Check MCP tools | DOM-aware reading, JS execution, navigation, page text extraction |
| Tier 3 | `obsidian:defuddle` | Check available skills | Clean markdown extraction, strips nav/ads, saves tokens |
| Tier 4 | WebFetch + WebSearch | Always available | Raw HTML fetch + general search (may miss JS content) |

**Fallback logic**: If Tier 1 fails or is unavailable → try Tier 2 → try Tier 3 → use Tier 4. Never stop at a failed tier — always fall through.

### 1-2. Primary Page Analysis

Fetch the provided URL and extract:

- **Product name** and tagline
- **One-line description**: what it does in plain language
- **Core feature list**: every distinct action a user can take
- **Target audience**: who this is built for
- **Input/Output model**: what users provide, what they receive
- **UI pattern**: quiz, calculator, dashboard, form-wizard, report generator, chatbot, editor, etc.
- **Tech signals**: frameworks visible in source, API patterns, third-party integrations
- **Pricing model**: free, freemium, paid, usage-based

If using Playwright or Chrome MCP, also capture:
- **Screenshots** of key screens (landing, main tool, results)
- **Interactive flow**: actually step through the tool to map the full UX sequence
- **Dynamic content**: anything loaded via JavaScript that static fetch would miss

**Failure recovery**:
- URL inaccessible → Search `"[product name]"` + `site:web.archive.org` or use WebSearch for cached/review content
- JavaScript-heavy SPA with blank static fetch → Fall to Playwright/Chrome tier, or analyze visible HTML meta + search results
- Rate limited or CAPTCHA → Use WebSearch to find blog posts, reviews, and documentation about the product instead

### 1-3. Similar Services Research

Don't just study the target product in isolation. Find and analyze 2-3 similar services to understand the broader landscape and extract the best ideas.

**How to find similar services:**
- `"[product name]" alternative` or `"[product name]" vs`
- `"best [category] tools [year]"` (e.g., "best idea validation tools 2026")
- `"[product name]" OR "[similar term]" site:producthunt.com`
- If `claude-seo:seo-dataforseo` is available: SERP competitors for the same keywords

**For each similar service, extract:**
- **What they do differently**: unique angles, features, UX approaches
- **What they do better**: strengths worth borrowing
- **What they do worse**: weaknesses to avoid
- **Pricing/positioning gap**: where the market has room

**How this improves the generated skill:**
- Borrow the best UI patterns across multiple products, not just one
- Scoring models informed by how the whole category evaluates things
- Feature set that cherry-picks strengths from several sources
- Avoid blind spots that come from studying only one product

Keep this lightweight — 15-20 min max. The goal is broader perspective, not exhaustive competitive analysis.

### 1-4. Subpage Exploration (Target Product)

Don't just read the landing page. Explore links that reveal the product's depth:

- **How it works / About**: the product narrative and methodology
- **Pricing**: business model reveals what features are core vs premium
- **FAQ / Help docs**: edge cases and common user confusions
- **Blog / Changelog**: recent updates show what the team prioritizes
- **Demo / Examples**: concrete use cases reveal the intended workflow
- **API docs** (if any): technical capabilities and data models

### 1-5. Community & Social Research

This is where you understand whether the product actually resonates and why. Run **parallel research tracks**:

**Track A — Quantitative Signals** (breadth):
- `"[product name]" site:producthunt.com` — upvotes, launch reception, maker comments
- `"[product name]" review OR alternative OR pricing` — competitive landscape
- `"[product name]" tutorial OR "how to use"` — real usage patterns
- If `claude-seo:seo-dataforseo` is available: keyword data, SERP position, traffic estimates

**Track B — Qualitative Signals** (depth):
- `"[product name]" site:twitter.com OR site:x.com` — real user reactions, viral moments
- `"[product name]" site:reddit.com` — detailed discussions, complaints, workarounds
- `"[product name]" site:news.ycombinator.com` — technical community perspective, architecture discussions

**What to extract from community research:**
- **Praise patterns**: what do users love? (replicate these carefully)
- **Complaint patterns**: what frustrates users? (improve on these)
- **Feature requests**: what's missing? (potential value-adds)
- **Use cases**: how do real people actually use it? (may differ from marketing)
- **Competitive mentions**: what alternatives exist and how does this compare?

**Failure recovery**:
- Product too new / no community mentions → Focus on the product's own content + similar products' community signals
- Site-specific search blocked (e.g., producthunt.com) → Use generic search `"[product name]" review` or fetch alternative review aggregators

### 1-6. Compile Analysis & Replicability Score

Organize all findings using the structure in `references/analysis-template.md`. Then assign a replicability score:

| Score | Meaning | Action |
|-------|---------|--------|
| 90-100% | Nearly everything replicable. Skill may improve on original. | Proceed with full build |
| 60-89% | Core value replicable. Some features need adaptation. | Tell user what changes, proceed |
| 30-59% | Significant portions can't replicate. | Ask user if feasible subset is worth building |
| Below 30% | Fundamentally not suited for skill conversion. | Explain why, suggest alternatives |

**Quality gate — Phase 1 → Phase 2**:

| Criterion | Pass condition |
|-----------|---------------|
| Product understood | One-line description + feature list + target audience documented |
| Research depth | At least 3 sources beyond the product's own site |
| Replicability assessed | Score assigned with specific feature-by-feature breakdown |
| Limitations identified | Every unreplicable feature listed with reason |

If any criterion fails: go deeper on that area before proceeding. Max 2 retry cycles before asking the user for guidance.

---

## Phase 2: User Consultation

Before building anything, consult the user. Different products warrant different output formats.

### Questions to Ask

Use AskUserQuestion (or ask directly) to clarify:

**1. Output format** — "What format would be most useful?"
- **Interactive React artifact**: best for quizzes, calculators, scoring tools, dashboards — renders directly in Claude
- **Standalone HTML file**: best to open in a browser, share, or host independently
- **Conversational skill only**: best for content generators, analysis tools, advisory services
- **Full package (skill + artifact)**: maximum flexibility

**2. Depth of replication** — "How closely should this match the original?"
- **Core functionality only**: fast, focused on the main value proposition
- **Full feature parity**: recreate everything the original does
- **Improved version**: replicate + fix complaints found in community research

**3. Additional context** (only if relevant)
- Any specific features to prioritize or skip?
- Any customizations for their specific use case?
- Language preference for the generated skill's UI?

Adapt the build plan based on answers. Don't ask unnecessary questions — if the product is clearly a simple calculator, you don't need to ask about dashboards.

**Quality gate — Phase 2 → Phase 3**:

| Criterion | Pass condition |
|-----------|---------------|
| User preferences captured | Output format + depth decided |
| Build plan adjusted | Plan reflects user's choices |

---

## Phase 3: Skill Design

### 3-1. Service Type Classification

| Type | Examples | Deliverables |
|------|----------|-------------|
| **Interactive tool** | Quiz, calculator, scorecard, diagnostic | SKILL.md + React/HTML artifact |
| **Data visualization** | Dashboard, report builder, tracker | SKILL.md + React artifact (recharts, d3) |
| **Content generator** | Copywriter, email writer, template engine | SKILL.md (conversational) |
| **Editor/Analyzer** | Writing checker, code reviewer, linter | SKILL.md + React artifact with live feedback |
| **Research/Audit** | Market analysis, SEO audit, competitor analysis | SKILL.md + references/ |
| **Hybrid** | Combination of above | Mix deliverables as needed |

### 3-2. Skill Structure

```
generated-skill-name/
├── SKILL.md              # Always — the brain of the skill
├── references/           # When complex models or templates exist
│   ├── scoring-model.md  # Scoring criteria, evaluation rubrics
│   └── templates.md      # Output templates
└── assets/               # When artifacts are needed
    ├── Component.jsx     # Interactive React artifact
    └── tool.html         # Standalone HTML tool
```

**No backend needed.** React .jsx artifacts render directly in Claude. Standalone .html files open in any browser. Keep everything client-side and stateless.

### 3-3. Naming Convention

Use a descriptive, functional name — never the original product's trademark.

Examples:
- A startup idea scoring tool → `idea-validator`
- A readability analysis app → `writing-clarity-checker`
- A booking link generator → `meeting-scheduler`
- A multi-step survey builder → `interactive-survey-builder`

### 3-4. Architecture Decision

Before coding, document the key design decisions:

```markdown
## Architecture Decisions
- **Why this UI pattern**: [quiz/calculator/dashboard] because [reasoning from research]
- **Scoring model basis**: [where the criteria come from — original product + community feedback]
- **Key improvement over original**: [what community complaints we're addressing]
- **What's adapted vs faithful**: [features changed and why]
- **What's excluded**: [features that can't be replicated, with honest explanation]
```

**Quality gate — Phase 3 → Phase 4**:

| Criterion | Pass condition |
|-----------|---------------|
| Service type classified | Clear deliverable list |
| Structure decided | File tree documented |
| Architecture justified | Key decisions documented with reasoning |
| Naming appropriate | Functional name, no trademark use |

---

## Phase 4: Skill Generation

### 4-1. Writing the SKILL.md

The SKILL.md is the brain of the generated skill. It should be thorough enough that Claude can deliver the product's full value through conversation alone, even without artifacts.

**Frontmatter:**
- `name`: functional, descriptive identifier
- `description`: what the skill does + trigger keywords. Be generous with triggers — undertriggering is the common failure mode. Include formal and casual phrasings.

**Body structure:**
1. One-line summary of what this skill does
2. Workflow diagram (input → processing → output)
3. Detailed step-by-step instructions for each phase
4. Output format / templates with concrete examples
5. Scoring models or business logic (reference external files for complex models)
6. Edge case handling
7. What to do when the skill can't fully solve the problem

**Quality bar:** The SKILL.md should read like documentation written by someone who deeply understands the product — not a surface-level summary. Include the *reasoning* behind design choices, not just the instructions.

### 4-2. Leveraging Available Tools (Cascading Fallback)

The generated skill should be smart about using whatever tools are available. Write the skill to check for and use these:

**For data collection / research tasks:**
- `claude-seo:seo-*` skills — SEO analysis, competitor research, content analysis
- `claude-seo:seo-dataforseo` — live SERP data, keyword metrics, backlink profiles
- `obsidian:defuddle` — clean web page extraction

**For interactive outputs:**
- `pw:playwright-pro` — browser automation, screenshots, testing
- Chrome MCP tools — page interaction, form filling, navigation

**For content & document generation:**
- `docx` skill — Word documents
- `pdf` skill — PDF generation
- `xlsx` skill — spreadsheets
- `pptx` skill — presentations

**For analysis & planning:**
- `product-management:*` — product specs, competitive briefs
- `marketing-skills:*` — marketing analysis, CRO, content strategy
- `finance-skills:*` — financial modeling

**Embed this pattern in every generated skill:**
```markdown
## Tool Usage (Cascading Fallback)

For [specific task], use the best available tool:
1. **[Best option]** — if [MCP/skill name] is available, use it because [reason]
2. **[Good option]** — if [alternative] is available, use it as fallback
3. **Otherwise**: [baseline approach that always works]
```

### 4-3. Interactive React/HTML Artifacts

When the product is interactive, create a production-quality React component or standalone HTML.

**React (.jsx) rules:**
- Single file with default export
- Tailwind CSS utility classes only (no custom CSS build needed)
- State management via React hooks (useState, useReducer)
- No localStorage — all data in memory
- Available libraries: recharts, lucide-react, d3, lodash, three.js, papaparse, shadcn/ui, chart.js, tone
- Mobile responsive (mobile-first approach)

**Quality expectations — match a finished product, not a prototype:**
- Clean visual hierarchy and professional typography
- Smooth transitions and micro-interactions where appropriate
- Accessible: keyboard navigation, sufficient contrast, ARIA labels, semantic HTML
- Error states and empty states handled gracefully
- Results presentation with charts/scores/breakdowns where relevant
- Export/download functionality where it makes sense

**Implementation priorities (in order):**
1. Core interaction logic (scoring, calculation, analysis) — must be faithful to the original
2. Visual design — polished, intentional, modern
3. Results presentation — clear, actionable
4. Accessibility — keyboard nav, contrast, screen reader friendly
5. Micro-interactions — smooth transitions, loading states

**Performance checklist for artifacts:**
- No unnecessary re-renders (use `memo`, `useCallback` where appropriate)
- Large lists virtualized if > 100 items
- Images optimized or SVG-based
- No blocking operations in render path

### 4-4. Scoring Models & Business Logic

When the original product scores, ranks, or evaluates, create `references/scoring-model.md` with:

- Every evaluation dimension with clear definitions
- Weight distribution and rationale
- Scoring criteria per level (what does a 3/10 vs 8/10 look like?)
- Grade/tier classification thresholds
- Result interpretation guide
- Concrete examples at different score levels

This separation keeps the SKILL.md readable while allowing deep-dive reference when needed.

**Quality gate — Phase 4 (internal, before delivery)**:

| Criterion | Pass condition |
|-----------|---------------|
| SKILL.md complete | All 7 body sections present |
| Triggers comprehensive | Both formal and casual phrasings included |
| Artifact renders | If artifact exists: mentally walk through a real scenario — does it work? |
| Scoring faithful | If scoring: criteria match or improve on original |
| Tool fallback embedded | Generated skill has its own cascading fallback logic |
| No trademark use | Skill name and content use functional names only |

---

## Phase 5: Verification & Delivery

### 5-1. Verification Checklist

Before delivering, verify the skill works. This is not optional — evidence over claims.

| Check | How to verify |
|-------|--------------|
| SKILL.md walkthrough | Read it and mentally walk through a real user scenario end-to-end |
| Artifact rendering | If HTML/JSX exists: save to file, verify it renders without errors |
| Script execution | If scripts exist: run them, confirm they execute cleanly |
| Feature coverage | Cross-reference against Phase 1 feature list — are all major features covered? |
| Scoring accuracy | If scoring model: run a sample input through and verify reasonable results |
| Limitation honesty | All unreplicable features explicitly documented |
| Quick test | Use the skill yourself with a sample input to confirm quality |

**Failure recovery during verification:**
- Artifact has rendering errors → Fix and re-verify (max 3 attempts)
- Scoring model produces unreasonable results → Revise weights/criteria and retest
- Missing features discovered → Add if within scope, or document as limitation
- If issues persist after 3 fix cycles → Deliver with known issues documented

### 5-2. Package & Deliver

Save everything to the workspace folder with clear organization. Present files using `present_files` if available.

**Deliver to user with:**

1. **Original service summary**: what was analyzed, research depth, tools used
2. **What was built**: list of generated files with brief descriptions
3. **How to use it**: installation instructions, example prompts to invoke the skill
4. **Differences from original**: what was replicated, what was improved, what couldn't be done
5. **Community insights**: interesting findings from X/Reddit/HN that shaped the design
6. **File links**: clickable links to all generated files

---

## Honesty About Limitations

Not everything can be cloned. Be upfront — it builds trust and saves time.

### Feature Replicability Guide

**Cannot replicate — tell user immediately:**
- Real-time data feeds (stock prices, live analytics, weather)
- Multi-user collaboration (shared cursors, real-time sync)
- Persistent user accounts and databases
- Payment processing or financial transactions
- Native mobile features (push notifications, camera, GPS)
- Third-party OAuth integrations that actually connect
- Sustained server infrastructure (webhooks, cron jobs, queues)

**Partially replicable — explain the adaptation:**
- API-dependent features → Claude's native analysis + user-provided data
- User history / saved sessions → in-memory state (works within session, not across)
- Complex ML models → Claude can approximate many NLP/classification tasks
- Real-time collaboration → single-user version that exports shareable results

**How to communicate — be specific, not vague:**

> "The original [Product] uses a live stock price feed for real-time portfolio risk. Since we can't connect to live market data, the skill asks you to paste your current portfolio values and calculates risk from that snapshot. The analysis logic is identical — only the data input method changes."

> "This product requires a persistent database for user accounts and saved projects. The skill version works within a single session — your work lives in the conversation and can be exported as files, but there's no login or save/load across sessions."

If the product is fundamentally impossible to replicate (primarily a social network, marketplace, or real-time multiplayer), say so clearly and offer to build the feasible subset.

---

## Important Principles

- **Reconstruct the value, not the code.** Analyze publicly visible features and UX to build a new implementation that delivers the same value. Never copy source code.
- **Replace paid APIs with Claude's capabilities.** If the original uses a paid NLP API, Claude can do the analysis conversationally. If it uses a paid data source, design the skill to accept user-provided data.
- **Respect trademarks.** Never use the original product's name as the skill name. Use functional, descriptive names.
- **Improve where possible.** If community research reveals consistent complaints, address them. If the original is limited in a way Claude can improve, do it.
- **Be transparent about limitations.** If features genuinely can't be replicated, say so clearly.
- **Use available tools intelligently.** Always use the best available tool, and make sure generated skills also embed fallback logic so they're resilient across different environments.
- **Evidence over claims.** Every quality assertion requires proof. Don't say "this works" — demonstrate it.
- **Context carries forward.** No phase starts cold. Every phase receives the full context from previous phases.
- **Fail fast, fix fast.** Maximum 3 retry cycles per issue before escalating to the user or documenting as a known limitation.
