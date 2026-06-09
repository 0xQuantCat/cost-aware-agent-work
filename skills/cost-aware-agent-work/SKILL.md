# Cost-Aware Agent Work

Use this skill when the user wants an AI agent to complete a coding, research, writing, debugging, or operational task while controlling usage, context bloat, unnecessary output, unnecessary loops, and overuse of premium reasoning.

This skill is model-agnostic. It does not require external tools, API keys, shell scripts, network access, telemetry, or repo-specific knowledge.

It can be used manually, inside a custom harness, or alongside optional routing tools such as Switchboard or any similar model-routing system. Do not assume such a tool is installed.

## Core Rule

```text
Classify the difficulty first.
Use the strongest reasoning only to reduce uncertainty.
Use cheaper or lower-effort execution once the task path is narrow.
Escalate only when ambiguity remains.
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
router or switchboard-style model selection
```

Do not use this skill for very small one-shot tasks unless the user explicitly asks for cost discipline.

## Operating Loop

Follow this loop:

```text
1. Classify the current phase.
2. Select the cheapest reliable reasoning/model level.
3. Plan only when planning reduces uncertainty.
4. Execute bounded work compactly.
5. Verify with mechanical extraction when possible.
6. Escalate only on ambiguity or risk.
7. Handoff concisely.
```

Important:

```text
Route each phase, not just the whole task.
```

A task may require premium reasoning for planning and cheap execution afterward.

## Difficulty Ladder

Use this 1-5 scale to decide effort.

```text
1 trivial:
  typos, formatting, tiny rewrites, simple extraction
  use cheapest / low effort

2 easy:
  clear local edits, obvious log parsing, small cleanup
  use cheap / low effort

3 normal:
  bounded implementation, known files, ordinary debugging
  use balanced / medium effort

4 hard:
  unclear failures, multi-file reasoning, public API risk
  use strong / high effort

5 expert:
  architecture, ambiguous strategy, mergeability risk, irreversible choices
  use frontier / premium effort
```

If unsure between two levels, choose the lower level for reversible work and the higher level for irreversible or high-risk decisions.

## Pricing Mental Model

If the current provider exposes cached input pricing, treat stable repeated context as valuable.

Example mental model:

```text
uncached input:  normal cost
cached input:    cheaper when supported
output:          usually the expensive part
```

Operational implications:

```text
keep stable context stable
put reusable instructions before dynamic task details
avoid unnecessary narration
prefer compact artifacts over long explanations
route trivial/easy work away from premium models
escalate reasoning only when uncertainty remains
```

Do not claim exact savings unless the user provides actual provider pricing and usage telemetry.

## Optional Router Compatibility

This skill is a routing policy, not a router.

It may be used with:

```text
manual model selection
a custom local harness
Switchboard
any similar paid or free model-router tool
```

Do not require or assume any specific router.

Manual mode:

```text
the user or agent classifies task difficulty
the user or agent chooses the model/reasoning level
```

Router-assisted mode:

```text
a routing layer maps difficulty levels to models
the agent still follows this skill's output, escalation, and stop policy
```

Hybrid mode:

```text
automatic routing for levels 1-3
explicit approval or high reasoning for levels 4-5
```

If a router is used, do not let it become an unbounded loop. Keep stop conditions, compact handoffs, and escalation reasons.

## Phase 0 — Classify

Before planning or executing, classify the current phase.

Return:

```text
classification:
- difficulty: 1-5
- phase: plan / execute / verify / escalate / handoff
- recommended effort: cheap / medium / high / premium
- reason
```

Keep this short.

Do not classify the entire task once and reuse that forever. Reclassify when the phase changes.

## Phase 1 — Plan

Use premium/high reasoning only for planning when the task is ambiguous, risky, or multi-step.

Return a compact execution card.

```text
Execution card:
- goal
- difficulty level: 1-5
- likely files/sources
- files/sources not to touch
- implementation or research path
- risks
- verification command or evidence standard
- definition of done
- fallback if blocked
- escalation triggers
```

Rules:

```text
do not implement during planning
do not produce a long essay
do not broaden scope unless needed
do not plan again unless blocked or new evidence changes the task
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
do not escalate just because premium reasoning is available
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

Stay in cheap/medium/lower effort if:

```text
target files/sources are known
failure is local
test output is clear
patch is small
next action is obvious
task is reversible
```

Escalate to high/premium reasoning if:

```text
architecture choice is unclear
multiple subsystems conflict
tests fail for unclear reasons
public API or irreversible behavior is involved
wrong abstraction risk is high
review/mergeability risk is high
security, data loss, or production risk is involved
```

Every escalation must include:

```text
- why lower effort is insufficient
- what decision needs stronger reasoning
- what output is expected from the escalation
```

## Phase 5 — Handoff

Use a compact final handoff.

```text
Handoff:
- task
- difficulty level used
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
routing policy
escalation rules
```

Dynamic suffix:

```text
current task
latest error
changed files
fresh test output
specific question
current difficulty rating
```

Do not constantly rewrite the stable prefix.

Stable context is what caching rewards when prompt caching is supported.

## Budget Header Template

Users can prepend this to agent tasks:

```text
Budget discipline:
- classify task difficulty before choosing effort
- one planning pass only unless blocked
- keep outputs compact
- do not narrate routine work
- use selected files/sources only
- summarize logs before reasoning over them
- route trivial/easy work to cheaper modes when available
- escalate only if ambiguity remains
- final response must be a concise handoff
```

## Stop Conditions

Stop and ask for clarification, approval, or a higher-level decision when:

```text
the task scope expands beyond the execution card
the agent needs to touch files/sources marked do-not-touch
tests fail for unclear reasons after one bounded fix attempt
the next action changes public API or production behavior
the agent cannot verify the result
the agent would need broad context rereads to continue
```

Do not keep looping just because progress seems possible.

## Anti-Patterns

Avoid:

```text
highest reasoning for every step
one model for every phase
broad context rereads
long final summaries
uncached prompt churn
debugging without a plan
premium model for log cleanup
verbose reasoning during execution
blind automation loops
treating cumulative billed tokens as current context
treating advertised context window as guaranteed usable budget
```

## One-Line Summary

```text
Route by difficulty, plan with premium reasoning, execute bounded work cheaply, and escalate only when ambiguity remains.
```
