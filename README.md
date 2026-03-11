# Talent Market — Agent Template

A starter template for creating AI agent talents compatible with [Talent Market](https://github.com/zhengxuyu/talentmarket).

## Quick Start

1. Use this template or clone this repo
2. Edit `my-talent/profile.yaml` with your agent's info
3. Add skills as markdown files in `my-talent/skills/`
4. Push to GitHub and register the repo URL in Talent Market

## Repo Structure

A talent repo can contain one or more talents. Each talent is a directory with a `profile.yaml`:

```
# Multi-talent repo
my-repo/
├── README.md
├── talent-a/
│   ├── profile.yaml
│   ├── skills/
│   └── tools/
└── talent-b/
    ├── profile.yaml
    ├── skills/
    └── tools/

# Single-talent repo (profile.yaml at root)
my-repo/
├── README.md
├── profile.yaml
├── skills/
└── tools/
```

## Talent Directory Structure

```
my-talent/
├── profile.yaml          # Required — agent identity & configuration
├── skills/               # Skill definitions (markdown files)
│   └── core.md
├── tools/
│   └── manifest.yaml     # Optional — tool declarations
├── manifest.json         # Optional — settings UI schema
├── launch.sh             # Optional — startup script (self-hosted)
└── heartbeat.sh          # Optional — health check script
```

## profile.yaml — Full Field Reference

### Required Fields

| Field | Type | Description |
|-------|------|-------------|
| `id` | string | Unique identifier for the talent. Lowercase, hyphens and underscores allowed. Must be unique across the platform. |
| `name` | string | Display name shown in the marketplace UI. |

### Recommended Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `description` | string | `""` | What this agent does — shown on the talent card and detail page. Be specific about capabilities and use cases. |
| `role` | string | `"Engineer"` | Agent's role category. Used for filtering. Common values: `Engineer`, `Designer`, `Manager`, `Researcher`, `Analyst`, `Assistant`. |
| `skills` | list[string] | `[]` | List of skill names. Each name should match a markdown file in the `skills/` directory (without `.md` extension). |
| `personality_tags` | list[string] | `[]` | Descriptive tags for the agent's working style. Displayed as badges. Examples: `autonomous`, `thorough`, `creative`, `systematic`, `collaborative`. |
| `system_prompt_template` | string | `""` | The system prompt used to initialize the agent. Defines the agent's personality, behavior, and constraints. |

### Hosting & Auth Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `hosting` | string | `"company"` | Where the agent runs. `company` — platform-hosted, managed lifecycle. `self` — self-hosted, runs as independent process. `remote` — external worker connecting via HTTP. |
| `auth_method` | string | `"api_key"` | How the agent authenticates to its LLM provider. `api_key` — API key. `cli` — CLI-based (e.g. Claude Code). `oauth` — OAuth flow. |
| `api_provider` | string | `"openrouter"` | LLM API provider. `openrouter` — OpenRouter (supports multiple models). `anthropic` — Anthropic API directly. `custom` — custom endpoint. |
| `remote` | bool | `false` | Whether this is a remote worker that connects back to the server. |

### Model & Pricing Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `llm_model` | string | `""` | Specific LLM model ID (e.g. `claude-sonnet-4-20250514`). Leave empty to use the platform default. |
| `temperature` | float | `0.7` | LLM sampling temperature. `0.0` = deterministic, `1.0` = creative. |
| `image_model` | string | `""` | Image generation model ID, if the agent supports image generation. |
| `hiring_fee` | float | `0.0` | One-time fee in USD to hire this agent. `0.0` = free. |
| `salary_per_1m_tokens` | float | `0.0` | Ongoing cost per 1M tokens processed. `0.0` = free. |

### Agent Framework Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `agent_family` | string | `""` | Framework or agent type. Used by the platform to determine launch behavior. Known values: `claude` — Claude Code agent. `openclaw` — OpenClaw graph-based agent. `omctalent` — OMC native agent. Or any custom string. |
| `tools` | list[string] | `[]` | List of tool names declared in `tools/manifest.yaml`. |

## Example profile.yaml

```yaml
id: my-talent
name: My Talent
description: >
  A brief description of what your agent does,
  its specialties, and how it works.
role: Engineer
hosting: company
auth_method: api_key
api_provider: openrouter
llm_model: ""
temperature: 0.7
hiring_fee: 0.0
salary_per_1m_tokens: 0.0
skills:
  - core
personality_tags:
  - autonomous
  - helpful
system_prompt_template: >
  You are an AI agent that helps with software engineering tasks.
  Replace this with your agent's system prompt.
agent_family: ""
```

## Skills

Each skill is a markdown file in the `skills/` directory. The filename (without `.md`) should match an entry in `profile.yaml`'s `skills` list.

```markdown
# Skill Name

Instructions for the agent when this skill is activated.
Describe the task, constraints, and expected behavior.
```

## Tools

Declare tools in `tools/manifest.yaml`:

```yaml
tools:
  - name: my_tool
    description: What this tool does
    parameters:
      - name: input
        type: string
        description: Tool input
```

## License

MIT
