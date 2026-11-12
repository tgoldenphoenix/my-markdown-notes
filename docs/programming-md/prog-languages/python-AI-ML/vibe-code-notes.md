# Vibe Coding Notes

## Jargon

An `agentic coding platform` is a software development environment powered by autonomous AI agents that can plan, write, test, debug, and execute multi-step coding tasks from high-level instructions with minimal human intervention. Example: Claude Code, Antigravity

you pay for the model (its token).

Claude Code is CLI tool

## Models

k

## Context

skills/: subdirectories containing a SKILL.md file with instructions for the agent.

rules/: markdown files defining behavioral constraints or style guidelines.

## What's MCP, SLOP and A2A and how can I benefit from it?

An `MCP Server` (Model Context Protocol Server) is a lightweight intermediary program that securely connects AI applications to external data sources, tools, and workflows.

## Plugins

Antigravity's `plugins` package reusable skills, Model Context Protocol (MCP) servers, custom agents and subagents, rules, and lifecycle hooks into a single versioned unit. 

## Tools

k

## Tokens

AI models don't work directly with words like "hello" or "world" either. Instead, they break everything down into smaller chunks called tokens.

Output tokens typically cost 2-4x more than input tokens, because generating new content requires more computational work than just processing what you sent.

You’ll want to be intentional about how much information you include in your initial context, which we’ll talk about next, and how you steer the model to be concise or detailed in its responses.

## Anti gravity

Planning mode has moved to `/plan`. Use /plan to explicitly ask your agent to generate a structured plan before implementation.

## Agent Skills

At its core, a skill is a folder containing a `SKILL.md` file. This file includes metadata (name and description, at minimum) and instructions that tell an agent how to perform a specific task. Skills can also bundle scripts, reference materials, templates, and other resources.

```txt
my-skill/
├── SKILL.md          # Required: metadata + instructions
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
├── assets/           # Optional: templates, resources
└── ...               # Any additional files or directories
```

The `SKILL.md` file must contain `YAML` frontmatter followed by Markdown content.

Gemini API skills follow the `open Agent Skills specification`, which works across editors and command-line agents such as Antigravity, Claude Code, Cursor, and Codex.

---

When you use a skill in your development environment, the coding agent processes the bundle in three stages:

1. Startup discovery: The agent reads only the YAML frontmatter (`name` and `description`) from each installed SKILL.md file and adds the short descriptions to its system context.
2. On-demand activation: When your prompt matches a skill description, the agent loads the full `SKILL.md` file into its context window to read the SDK rules and code templates.
3. Resource execution: When a task requires detailed parameters or media processing, the instructions in SKILL.md direct the agent to read local guides in references/, run helper scripts in scripts/, or fetch live documentation pages from aistudio.google.com/docs.

---

Skills are one of several options you can use to give your agent additional context, alongside system prompts (e.g. `AGENTS.md`) and function-calling tools. Each option has a specific use case and affects the context window differently. For example, an `AGENTS.md` file is included on every conversation turn, so putting a large SDK reference into `AGENTS.md` consumes context tokens on every turn even when you edit unrelated frontend or database code.

Context options:

- System prompts (e.g. AGENTS.md): Included on every conversation turn. Best for global repository rules, coding style conventions, and project architecture overview.
- Skills (SKILL.md bundles): Loaded on demand when relevant to the prompt. Best for domain-specific SDK patterns, API migration guides, multi-step workflows, and local helper scripts.
- Tools (Function calling): Loaded on demand during execution. Best for fetching live external data, querying databases, or triggering external side effects.