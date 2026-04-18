# MCP, Skill & Plugin Fallback Chains

This reference documents the cascading fallback logic for every major capability the url-to-skill converter needs. When generating skills, embed similar fallback patterns so the generated skill is resilient across different environments.

## Web Page Analysis

| Priority | Tool | How to detect | Capabilities |
|----------|------|--------------|-------------|
| 1 | `pw:playwright-pro` | Check available skills | Full browser automation, screenshots, JS rendering, form interaction, SPA support |
| 2 | `mcp__Claude_in_Chrome__*` | Check available MCP tools | DOM-aware reading, JS execution, navigation, page text extraction |
| 3 | `obsidian:defuddle` | Check available skills | Clean markdown extraction from URLs, strips clutter |
| 4 | WebFetch | Always available | Raw HTML fetch, may miss JS-rendered content |

## Search & Research

| Priority | Tool | How to detect | Capabilities |
|----------|------|--------------|-------------|
| 1 | `claude-seo:seo-dataforseo` | Check available skills | Live SERP data, keyword metrics, backlink profiles, AI visibility |
| 2 | WebSearch | Always available | General web search |
| 3 | `obsidian:defuddle` + URLs | Check available skills | Clean extraction of specific known URLs |

## Community Monitoring

| Priority | Tool | How to detect | Capabilities |
|----------|------|--------------|-------------|
| 1 | Slack MCP (`mcp__*__slack_*`) | Check available MCP tools | Read channels, search messages |
| 2 | WebSearch with site filters | Always available | `site:reddit.com`, `site:x.com`, `site:news.ycombinator.com` |

## Competitive Analysis

| Priority | Tool | How to detect | Capabilities |
|----------|------|--------------|-------------|
| 1 | `claude-seo:seo-dataforseo` | Check available skills | SERP analysis, competitor data |
| 2 | `product-management:competitive-brief` | Check available skills | Structured competitive analysis |
| 3 | `marketing-skills:competitor-alternatives` | Check available skills | Competitor comparison content |
| 4 | WebSearch | Always available | Manual research |

## Content Quality Analysis

| Priority | Tool | How to detect | Capabilities |
|----------|------|--------------|-------------|
| 1 | `claude-seo:seo-content` | Check available skills | E-E-A-T analysis, readability, content depth |
| 2 | `marketing-skills:copy-editing` | Check available skills | Copy quality review |
| 3 | Claude native analysis | Always available | Built-in text analysis |

## Document Generation

| Priority | Tool | How to detect | Capabilities |
|----------|------|--------------|-------------|
| 1 | `docx` skill | Check available skills | Professional Word documents |
| 2 | `pdf` skill | Check available skills | PDF generation |
| 3 | `xlsx` skill | Check available skills | Spreadsheets |
| 4 | `pptx` skill | Check available skills | Presentations |
| 5 | Markdown + HTML | Always available | Lightweight output |

## Screenshot & Visual Analysis

| Priority | Tool | How to detect | Capabilities |
|----------|------|--------------|-------------|
| 1 | `pw:playwright-pro` | Check available skills | Automated screenshots, visual testing |
| 2 | `mcp__computer-use__screenshot` | Check available MCP tools | Desktop screenshots |
| 3 | `claude-seo:seo-visual` | Check available skills | Visual analysis with Playwright |
| 4 | Describe from page text | Always available | Infer UI from HTML structure |

## Product Strategy Context

| Priority | Tool | How to detect | Capabilities |
|----------|------|--------------|-------------|
| 1 | `product-management:write-spec` | Check available skills | Feature specs, PRDs |
| 2 | `code-to-prd:code-to-prd` | Check available skills | Reverse-engineer requirements |
| 3 | `marketing-skills:customer-research` | Check available skills | Customer insights |
| 4 | Manual analysis | Always available | Structured analysis from research |

---

## How to Check Availability

In the skill execution context, the available skills and MCP tools are listed in the system prompt. Check for:

```
# Skills: listed in <available_skills> section
# MCP tools: check for tool names matching patterns above
# Fallback: always use WebSearch + WebFetch as baseline
```

## Embedding Fallback Logic in Generated Skills

When the url-to-skill converter creates a new skill, that skill should also have intelligent fallback. Use this pattern:

```markdown
## Tool Usage

This skill works best with [specific tool], but adapts to your environment:

1. **If [best tool] is available**: [what to do with it]
2. **If [good alternative] is available**: [what to do instead]  
3. **Otherwise**: [baseline approach that always works]
```

This ensures every generated skill is resilient regardless of the user's setup.
