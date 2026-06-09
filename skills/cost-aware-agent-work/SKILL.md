# Cost-Aware Agent Work

Use this skill when the user wants an AI agent to complete a coding, research, writing, debugging, or operational task while controlling usage, context bloat, unnecessary output, and overuse of premium reasoning.

This skill is model-agnostic. It does not require external tools, API keys, shell scripts, network access, or repo-specific knowledge.

## Core Rule

```text
Use the strongest reasoning only to reduce uncertainty.
Use cheaper or lower-effort execution once the task path is narrow.
```

Do not use the highest reasoning mode for every step by default.

## When To Use

Use this skill when the task involves any of:

```text
large context
long-running agent work
coding or debugging
multi-step implementation
research synthesis
test/log analysis
quota or token concerns
expensive reasoning modes
model-ladder decisions
```

Do not use this skill for very small one-shot tasks unless the user explicitly asks for cost discipline.

## Pricing Mental Model

If the current provider exposes cached input pricing, treat stable repeated context as valuable.

Example mental model:

```text
uncached input:  normal cost
cached input:    much cheaper
output:          usually the expensive part
```

Operational implications:

```text
keep stable context stable
put reusable instructions before dynamic task details
avoid unnecessary narration
prefer compact artifacts over long explanations
escalate reasoning only when uncertainty remains
```

Do not claim exact savings unless the user provides actual provider pricing and usage telemetry.

## Phase 1 — Plan

Use premium/high reasoning only for planning when the task is ambiguous, risky, or multi-step.

Return a compact execution card.

```text
Execution card:
- goal
- likely files/sources
- files/sources not to touch
- implementation or research path
- risks
- verification command or evidence standard
- definition of done
- fallback if blocked
```

Rules:

```text
do not implement during planning
do not produce a long essay
do not broaden scope unless needed
```

## Phase 2 — Execute

Use medium/lower-effort reasoning when the execution path is clear.

Follow the execution card.

Return only:

```text
- changes made or findings
- commands/evidence used
- result
- blockers
- remaining risks
```

Rules:

```text
do not narrate routine work
do not re-plan unless blocked
do not inspect broad context if target sources are known
do not produce speculative summaries during execution
```

## Phase 3 — Verify

Use the cheapest reliable model/reasoning level for mechanical verification, extraction, and cleanup.

For logs or test output, extract only:

```text
- failing test or command
- error type
- relevant file/line
- suspected cause
- next action
```

Avoid full-log summaries unless requested.

## Phase 4 — Escalate

Stay in medium/lower effort if:

```text
target files/sources are known
failure is local
test output is clear
patch is small
next action is obvious
```

Escalate to high/premium reasoning if:

```text
architecture choice is unclear
multiple subsystems conflict
tests fail for unclear reasons
public API or irreversible behavior is involved
wrong abstraction risk is high
review/mergeability risk is high
```

Every escalation should have a reason.

## Phase 5 — Handoff

Use a compact final handoff.

```text
Handoff:
- task
- files/sources changed or used
- commands/evidence checked
- tests passed/failed
- unresolved risks
- next step
```

Avoid long retrospectives unless explicitly requested.

## Cache-Stable Prompt Layout

Put stable instructions first.

Stable prefix:

```text
project rules
coding conventions
quality rubric
standing constraints
branch policy
known commands
preferred output format
```

Dynamic suffix:

```text
current task
latest error
changed files
fresh test output
specific question
```

Do not constantly rewrite the stable prefix.

Stable context is what caching rewards.

## Budget Header Template

Users can prepend this to agent tasks:

```text
Budget discipline:
- one planning pass only
- keep outputs compact
- do not narrate routine work
- use selected files/sources only
- summarize logs before reasoning over them
- escalate only if ambiguity remains
- final response must be a concise handoff
```

## Anti-Patterns

Avoid:

```text
highest reasoning for every step
broad context rereads
long final summaries
uncached prompt churn
debugging without a plan
premium model for log cleanup
verbose reasoning during execution
treating cumulative billed tokens as current context
treating advertised context window as guaranteed usable budget
```

## One-Line Summary

```text
Use the smartest model to make the task narrow, not to do every narrow step.
```
