<p align="center">
  <img src="logo-text.png" alt="AI Agent Talent Market" width="360" />
</p>

# Talent Market — Agent Template

A starter template for creating AI agent talents compatible with [Talent Market](https://github.com/zhengxuyu/talentmarket).

> **Complete Guide:** [How to publish your Agent to Talent Market](https://1mancompany.github.io/OneManCompany/docs/guide/publish-to-talent-market/) — step-by-step walkthrough covering profile.yaml, skills, tools, pricing, and promotion tips.

## Quick Start

1. Use this template or clone this repo
2. Edit `my-talent/profile.yaml` with your agent's info
3. Add skills as folders in `my-talent/skills/<name>/SKILL.md`
4. Add tools as folders in `my-talent/tools/<name>/TOOL.md`
5. Push to GitHub and register the repo URL in Talent Market

### Vibe Coding

We encourage using AI coders (Claude Code, Cursor, Copilot, etc.) to build and convert talents. When prompting your AI coder, include:

> Follow the instructions in `vibe-coding-guide.md` to convert this agent into the Talent Market template format.

See [`vibe-coding-guide.md`](./vibe-coding-guide.md) for the full conversion guide.

### Quick Recipe: Convert an Existing Agent

> **IMPORTANT:** Always create a **new separate repo** for each converted talent. Do NOT commit into this template repo — it should stay clean as a reference.

**From a Claude Code agent:**

```
Convert the agent at https://github.com/user/agent-repo into the Talent Market template format
(https://github.com/1mancompany/talent-template) following vibe-coding-guide.md.

Steps:
1. Create a new repo under my GitHub account
2. Create profile.yaml from CLAUDE.md (extract name, description, system prompt)
3. Split capabilities into skills/<name>/SKILL.md folders
4. Copy .mcp.json to tools/.mcp.json, create TOOL.md for each MCP server
5. Copy any other files or folders from the source
6. Add original repo citation to DESCRIPTION.md
7. Push to GitHub
```

**From an OpenClaw agent:**

```
Convert the agent at https://github.com/user/agent-repo into the Talent Market template format
(https://github.com/1mancompany/talent-template) following vibe-coding-guide.md.

Steps:
1. Create a new repo under my GitHub account
2. Create profile.yaml (set agent_family: openclaw, hosting: self)
3. Map each workflow node to a skills/<name>/SKILL.md folder
4. Copy MCP configs to tools/.mcp.json, keep launch.sh
5. Add original repo citation to DESCRIPTION.md
6. Push to GitHub
```

**From any other agent (LangChain, CrewAI, AutoGen, etc.):**

```
Convert the agent at https://github.com/user/agent-repo into the Talent Market template format
(https://github.com/1mancompany/talent-template) following vibe-coding-guide.md.

Steps:
1. Create a new repo under my GitHub account
2. Find the system prompt in the source code, create profile.yaml
3. Identify distinct capabilities, create skills/<name>/SKILL.md for each
4. List tools in tools/<name>/TOOL.md folders
5. Copy any other files or folders from the source
6. Add original repo citation to DESCRIPTION.md
7. Push to GitHub
```

## Repo Structure

A talent repo can contain one or more talents. Each talent is a directory with a `profile.yaml`:

```
# Multi-talent repo
my-repo/
├── README.md
├── talent-a/
│   ├── profile.yaml
│   ├── DESCRIPTION.md    # Agent description, demos, success stories
│   ├── avatar.jpg        # Each talent can have its own avatar
│   ├── skills/
│   │   └── core/
│   │       └── SKILL.md
│   └── tools/
└── talent-b/
    ├── profile.yaml
    ├── avatar.png
    ├── skills/
    │   └── core/
    │       └── SKILL.md
    └── tools/

# Single-talent repo (profile.yaml at root)
my-repo/
├── README.md
├── profile.yaml
├── avatar.jpg
├── skills/
│   └── core/
│       └── SKILL.md
└── tools/
```

## Talent Directory Structure

```
my-talent/
├── profile.yaml          # Required — agent identity & configuration
├── DESCRIPTION.md        # Optional — rich agent description, demos, success stories
├── avatar.jpg            # Optional — talent avatar (png/jpg/svg/webp, auto-loaded)
├── skills/               # Each skill is a folder with SKILL.md
│   └── core/
│       └── SKILL.md
├── tools/                # Each tool is a folder with TOOL.md
│   ├── .mcp.json         # MCP server definitions (standard format)
│   └── filesystem/
│       └── TOOL.md       # Tool description & usage docs
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
| `avatar` | string | `""` | Filename of the avatar image (e.g. `avatar.jpg`). Place the file in the same directory as `profile.yaml`. Supported formats: `.png`, `.jpg`, `.svg`, `.webp`. Auto-detected if named `avatar.*`. Displayed on talent cards in the marketplace. |
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

### Source Repository Fields

| Field | Type | Default | Description |
|-------|------|---------|-------------|
| `upstream_repo_url` | string | `""` | URL of the original open-source repository this agent is based on (e.g. `https://github.com/anthropics/claude-code`). Displayed as a link on the talent detail page. Omit or leave empty if not applicable. |
| `repo_stars` | int | `null` | GitHub star count of the upstream repo. Displayed on the talent card and detail page. Static value — update manually. Omit if not applicable. |

## Example profile.yaml

```yaml
id: my-talent
name: My Talent
avatar: avatar.jpg  # Replace with your own image (png/jpg/svg/webp)
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
upstream_repo_url: ""       # Optional — link to original repo
repo_stars:                 # Optional — star count (static, omit if none)
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

Each skill is a **folder** inside `skills/` containing a `SKILL.md` file. The folder name should match an entry in `profile.yaml`'s `skills` list. The `SKILL.md` must include YAML frontmatter with `name` and `description`:

```
skills/
├── core/
│   └── SKILL.md
└── code-review/
    └── SKILL.md
```

```markdown
---
name: core
description: Brief description of when this skill should be activated.
---

# Skill Name

Instructions for the agent when this skill is activated.
Describe the task, constraints, and expected behavior.
```

## Tools

Each tool is a **folder** inside `tools/` containing a `TOOL.md` and optionally a `manifest.yaml` and implementation code.

### MCP Tools

Place `tools/.mcp.json` in the standard format. Create a folder per MCP server with a `TOOL.md`:

```
tools/
├── .mcp.json               # Standard MCP server definitions
├── filesystem/
│   └── TOOL.md             # What this tool does, when to use it
└── github/
    └── TOOL.md
```

`tools/.mcp.json`:
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-filesystem"],
      "env": {}
    }
  }
}
```

Empty `env` values become secrets the user must configure after hiring.

### Custom Tools

Each custom tool gets a folder with `TOOL.md`, `manifest.yaml`, and implementation:

```
tools/
└── run-tests/
    ├── TOOL.md              # Usage docs (with frontmatter)
    ├── manifest.yaml        # Metadata & parameters
    └── run.sh               # Implementation
```

`tools/run-tests/TOOL.md`:
```markdown
---
name: run-tests
description: Execute the project test suite and report results.
---

# Run Tests
Runs the full test suite. Use after code changes to verify correctness.
```

`tools/run-tests/manifest.yaml`:
```yaml
name: run-tests
type: shell
command: bash run.sh
parameters:
  - name: filter
    type: string
    description: Test name filter pattern
    required: false
```

## Publishing to Talent Market

Your repo can be **public or private**.

**If your repo is private**, add the platform bot as a collaborator before submitting:

1. Go to your repo **Settings → Collaborators**
2. Add **`1mancompany-bot`** with **Read** permission
3. Then submit your repo URL in the Add Talent page

Buyers who purchase your talent are automatically added as collaborators to the platform's private fork — they get read access to the fork, not to your original repo.

> **Note:** After adding a collaborator, GitHub sends an invitation. Buyers need to accept the invitation before they can clone the repo.

## Troubleshooting

If you run into issues uploading your talent to Talent Market — scan failures, profile validation errors, or anything unexpected — please [open an issue](https://github.com/1mancompany/talent-template/issues) on this repo. Include:

- Your repo URL (or a minimal reproduction)
- The error message you received
- Steps to reproduce

We'll help you get published.

## License

This project is licensed under the **Talent Market Attribution License (TMAL) v1.0**.

You are free to use, modify, and distribute this template commercially, provided you
retain the Citation section below in your README. See [LICENSE](./LICENSE) for full terms.

---

## Citation

> **DO NOT REMOVE THIS SECTION** — Required by the [Talent Market Attribution License](./LICENSE).

This talent was built using the [Talent Market](https://one-man-company.com) template by [Zhengxu Yu](mailto:yuzxfred@gmail.com) / [1mancompany](https://github.com/1mancompany).

```
@software{talentmarket,
  title  = {Talent Market - AI Agent Marketplace},
  author = {Zhengxu Yu},
  email  = {yuzxfred@gmail.com},
  url    = {https://one-man-company.com},
  year   = {2026}
}
```

If you publish or deploy a talent based on this template, please keep this section
intact in your README or equivalent documentation.
