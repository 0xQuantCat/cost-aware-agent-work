# Cost-Aware Agent Work

A small, model-agnostic skill/rulebook for reducing waste in AI agent workflows.

It teaches agents to:

```text
plan with premium reasoning
execute bounded work with cheaper reasoning
control output
preserve cache-stable context
escalate only on ambiguity
produce compact handoffs
```

## Why this exists

Many users burn through weekly AI usage by using the strongest reasoning mode for every phase of work:

```text
planning
searching
reading
editing
debugging
formatting
summarizing
```

This rulebook separates high-leverage reasoning from mechanical execution.

## Files

```text
skills/cost-aware-agent-work/SKILL.md
README.md
```

## Install

There is no executable installer. Review the file before use.

### Option A — Clone and copy the skill

```bash
git clone https://github.com/YOUR_USERNAME/cost-aware-agent-work.git
cd cost-aware-agent-work
```

Then copy:

```text
skills/cost-aware-agent-work/SKILL.md
```

into the skill/location supported by your agent tool.

### Option B — Use as a repo-level instruction

Copy the relevant rules into your project’s agent instruction file, such as:

```text
AGENTS.md
CLAUDE.md
.cursor/rules/
other tool-specific instruction files
```

Use the parts that match your workflow.

### Option C — Manual paste

Paste the `Budget Header Template` into the top of expensive agent tasks.

## Suggested article CTA

```text
I put the checklist in a small repo.

Clone it, read the SKILL.md, and copy it into your agent/tool setup.

No installer. No background process. Just a rulebook for cost-aware agent work.
```

## Safety

This repository should contain no executable automation by default.

Recommended constraints:

```text
no install script
no network calls
no shell hooks
no API keys
no telemetry
no hidden prompts
```

The skill is intentionally plain Markdown so users can inspect it before using it.

## License

MIT or CC0 recommended.
