# Vibe Coding Guide — Converting Agents to Talent Market Format

This guide explains how to convert an AI agent definition (typically a markdown file with YAML frontmatter) into a **Talent Market talent** following the `talent-template` structure.

## Output Checklist (ALL items required)

You MUST produce exactly **3 files** in the output directory. Do not skip any.

```
<talent-id>/
├── profile.yaml              ← REQUIRED (agent metadata)
├── skills/
│   └── core.md               ← REQUIRED (agent instructions — VERBATIM copy)
└── tools/
    └── manifest.yaml          ← REQUIRED (tool declarations template)
```

**Verify before finishing:** All 3 files exist? `profile.yaml` has all required fields? `skills/core.md` is an exact copy of the source body?

---

## Step 1: Determine the `talent-id`

- Use the source filename (without `.md` extension) as the talent ID
- Must be lowercase, hyphens allowed, no spaces
- Example: `engineering-senior-developer.md` → `engineering-senior-developer`

---

## Step 2: Create `skills/core.md` (DO THIS FIRST)

This is the most important file. It contains the agent's full instructions.

**Rule: VERBATIM COPY. Zero modifications.**

1. Take the source markdown file
2. Find the closing `---` of the YAML frontmatter
3. Copy EVERYTHING after it — every character, every emoji, every heading, every code block
4. Write it to `skills/core.md`

**DO NOT:**
- Remove or change emoji characters (e.g., keep `## 🧠 Your Identity` exactly as-is)
- Reformat, rewrite, summarize, or "clean up" any content
- Add or remove blank lines
- Change heading levels or wording

The content must be **byte-for-byte identical** to the source body. When in doubt, copy more rather than less.

---

## Step 3: Create `profile.yaml`

Extract metadata from the source frontmatter and fill in this template exactly:

```yaml
id: <talent-id>
name: <from source frontmatter "name" field>
description: >
  <from source frontmatter "description" field — keep original wording>
role: <specific job role — see Role Guidelines below>
hosting: self
auth_method: api_key
api_provider: anthropic
llm_model: ""
temperature: 0.7
hiring_fee: 0.0
salary_per_1m_tokens: 0.0
skills:
  - core
personality_tags:
  - <tag1>
  - <tag2>
system_prompt_template: >
  You are <name>. <first 1-2 sentences of description>
agent_family: ""
```

### Required Fields (must ALL be present and correct)

| Field | Value | Rule |
|-------|-------|------|
| `id` | `<talent-id>` | From source filename, lowercase with hyphens |
| `name` | `<display name>` | From source frontmatter `name`. If missing, derive from filename (convert hyphens to spaces, title case) |
| `description` | `<text>` | From source frontmatter `description`. Keep original wording exactly |
| `role` | `<job title>` | A specific job title from the Role Table below |
| `hosting` | `self` | Always `self` — never `company` |
| `api_provider` | `anthropic` | Always `anthropic` — never `openrouter` |
| `skills` | `[core]` | Always exactly `["core"]` |
| `system_prompt_template` | `<short>` | Format: `"You are <name>. <description summary>"` — max 2 sentences |

### Required Fields with Fixed Defaults

| Field | Value |
|-------|-------|
| `auth_method` | `api_key` |
| `llm_model` | `""` (empty string) |
| `temperature` | `0.7` (use `0.3` only if the agent does financial/compliance work requiring deterministic output) |
| `hiring_fee` | `0.0` |
| `salary_per_1m_tokens` | `0.0` |
| `agent_family` | `""` (empty string) |

### Recommended Fields

| Field | Rule |
|-------|------|
| `personality_tags` | 2-5 tags from the Personality Tags list below |

---

## Step 4: Create `tools/manifest.yaml`

Always use this exact content:

```yaml
# Tool manifest — declare tools this agent can use
# Uncomment and customize as needed

# builtin_tools:
#   - Read
#   - Write
#   - Bash
```

---

## Role Guidelines

Assign the **most specific job title** that matches what the agent actually does. Do NOT use generic labels.

| Agent Domain | Available Roles |
|-------------|----------------|
| Design | `Designer`, `UI Designer`, `UX Designer`, `UX Researcher`, `Brand Designer` |
| Engineering | `Engineer`, `Frontend Engineer`, `Backend Engineer`, `DevOps Engineer`, `SRE`, `Security Engineer`, `Data Engineer`, `Database Engineer`, `AI Engineer`, `ML Engineer`, `Mobile Engineer`, `Blockchain Engineer`, `Embedded Engineer`, `Software Architect` |
| Marketing | `Marketer`, `SEO Specialist`, `Content Creator`, `Content Strategist`, `Social Media Marketer`, `Growth Marketer`, `E-commerce Marketer`, `Community Manager`, `ASO Specialist` |
| Paid Media & Advertising | `Media Buyer`, `PPC Specialist`, `Programmatic Buyer`, `Ad Strategist` |
| Sales | `Sales Coach`, `Sales Strategist`, `Sales Engineer`, `Sales Analyst` |
| Product | `Product Manager`, `Product Researcher` |
| Project Management | `Project Manager`, `Producer`, `Operations Manager` |
| Game Development | `Game Engineer`, `Game Designer`, `Technical Artist`, `Narrative Designer`, `Audio Engineer`, `Level Designer` |
| Spatial / XR | `XR Engineer` |
| Testing & QA | `QA Engineer`, `Performance Engineer`, `Accessibility Tester` |
| Support & Operations | `Support`, `Data Analyst`, `Analyst`, `Operations` |
| Finance & Compliance | `Finance`, `Compliance Auditor`, `Compliance Specialist`, `Security Auditor` |
| Specialized | `Recruiter`, `Consultant`, `Advisor`, `Developer Advocate`, `Strategist`, `Training Designer`, `Technical Writer`, `Orchestrator`, `Content Creator` |

**How to pick:** Read the agent's name and description. Find the role that most precisely describes their primary function. For example:
- "PPC Campaign Strategist" → `PPC Specialist`
- "Programmatic Media Buyer" → `Programmatic Buyer`
- "App Store Optimization Specialist" → `ASO Specialist`
- "Inclusive Visuals Specialist" → `Designer`
- "Narrative Designer" → `Narrative Designer`

If nothing in the table fits precisely, use the closest match. Never fall back to just "Specialist".

---

## Personality Tags

Choose 2-5 from this list, based on the agent's described working style and personality:

| Tag | When to use |
|-----|-------------|
| `autonomous` | Works independently, self-directed |
| `systematic` | Follows structured processes, methodical |
| `creative` | Focuses on innovation, originality, artistic expression |
| `analytical` | Data-driven, metrics-focused, evidence-based |
| `collaborative` | Team-oriented, works across disciplines |
| `detail-oriented` | Precise, meticulous, pixel-perfect |
| `strategic` | Big-picture thinking, long-term planning |
| `thorough` | Comprehensive coverage, leaves nothing unchecked |
| `performance-focused` | Optimization-oriented, speed/efficiency matters |
| `security-focused` | Security/compliance-aware, risk-conscious |

---

## Common Mistakes — Read This

| # | Mistake | Correct Behavior |
|---|---------|-----------------|
| 1 | Modifying `skills/core.md` content (removing emojis, reformatting, summarizing) | Copy source body **exactly as-is**, including all emojis, formatting, code blocks |
| 2 | Forgetting to create `skills/core.md` | This is the MOST IMPORTANT file. Create it FIRST |
| 3 | Setting `hosting: company` | Must be `self` |
| 4 | Setting `api_provider: openrouter` | Must be `anthropic` |
| 5 | Using generic role like "Specialist" or "Engineer" | Use the most specific role from the Role Table |
| 6 | Making `system_prompt_template` too long | Keep it to 1-2 sentences max |
| 7 | Forgetting `tools/manifest.yaml` | Always create it, even though it's just a template |
| 8 | Wrong `id` (doesn't match directory name / source filename) | Must match exactly |

---

## Full Example

**Source file:** `marketing-seo-specialist.md`

```markdown
---
name: SEO Specialist
description: Expert in technical SEO, content optimization, and search strategy
color: blue
emoji: 🔍
vibe: Gets your site to page one and keeps it there.
---

# 🔍 SEO Specialist Agent

You are **SEO Specialist**, an expert in technical SEO...

## 🧠 Your Identity & Memory
- **Role**: Search engine optimization expert
...
```

### Output:

**`marketing-seo-specialist/profile.yaml`:**
```yaml
id: marketing-seo-specialist
name: SEO Specialist
description: >
  Expert in technical SEO, content optimization, and search strategy
role: SEO Specialist
hosting: self
auth_method: api_key
api_provider: anthropic
llm_model: ""
temperature: 0.7
hiring_fee: 0.0
salary_per_1m_tokens: 0.0
skills:
  - core
personality_tags:
  - analytical
  - strategic
  - thorough
system_prompt_template: >
  You are SEO Specialist. Expert in technical SEO, content optimization, and search strategy.
agent_family: ""
```

**`marketing-seo-specialist/skills/core.md`:**
```markdown
# 🔍 SEO Specialist Agent

You are **SEO Specialist**, an expert in technical SEO...

## 🧠 Your Identity & Memory
- **Role**: Search engine optimization expert
...
```

Note: The emojis `🔍` and `🧠` in the headings are **preserved exactly** from the source.

**`marketing-seo-specialist/tools/manifest.yaml`:**
```yaml
# Tool manifest — declare tools this agent can use
# Uncomment and customize as needed

# builtin_tools:
#   - Read
#   - Write
#   - Bash
```
