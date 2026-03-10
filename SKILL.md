---
name: self-evolve
description: |
  Self-evolving skill that analyzes yesterday's session histories every morning to discover improvement opportunities.
  Spots repeated patterns, unsatisfied answers, missing context, skill gaps, and recurring tool failures.
  Automatically creates new skills, references, commands, or rules. Logs every evolution.

  **When to use this Skill**:
  (1) Morning heartbeat or cron triggers daily session review
  (2) User asks to analyze recent sessions for improvement opportunities
  (3) User says "evolve", "self-improve", "analyze sessions", "what did I struggle with"
  (4) User wants to review the evolution log
---

# Self-Evolve

Analyze yesterday's sessions, find gaps, create improvements automatically.

## Daily Analysis Workflow

1. **Collect sessions** — Use `sessions_list` with `activeMinutes` to get yesterday's sessions, then `sessions_history` for each.

2. **Analyze patterns** — Look for these signals in the transcripts:
   - **Repeated patterns**: Same question/task asked across multiple sessions
   - **Unsatisfied answers**: User corrections, re-asks, "that's not what I meant"
   - **Missing context**: Agent had to ask for info that should be in USER.md, TOOLS.md, or MEMORY.md
   - **Skill gaps**: Tasks where no skill existed and agent had to improvise
   - **Tool failures**: Repeated errors, workarounds, or manual steps that could be automated
   - **Slow workflows**: Multi-step processes that could be scripted

3. **Generate improvements** — For each finding, decide the action:
   - **New skill**: Create via skill-creator if a reusable workflow emerged
   - **New reference**: Add to existing skill's `references/` if domain knowledge was missing
   - **Rule update**: Append to AGENTS.md or SOUL.md if a behavioral pattern should change
   - **Memory update**: Add to USER.md, TOOLS.md, or MEMORY.md if context was missing
   - **Heartbeat task**: Add to HEARTBEAT.md if a periodic check was needed

4. **Log the evolution** — Append to `evolutions/YYYY-MM-DD.md` with:
   - What was found (signal type + evidence)
   - What was created/changed (file path + summary)
   - Confidence level (high/medium/low)

## Analysis Prompt Template

When analyzing session history, use this mental framework. See `references/analysis-framework.md` for the detailed rubric.

## Evolution Log Format

Each entry in `evolutions/YYYY-MM-DD.md`:

```markdown
## Evolution #N — [Title]

- **Signal**: [repeated-pattern | unsatisfied-answer | missing-context | skill-gap | tool-failure | slow-workflow]
- **Evidence**: [Quote or description from session]
- **Action**: [created-skill | added-reference | updated-rule | updated-memory | added-heartbeat-task]
- **Target**: [file path that was created/modified]
- **Summary**: [One-line description of the change]
- **Confidence**: [high | medium | low]
```

## Cron Setup

To run self-evolve automatically each morning, add a cron job via OpenClaw CLI:

```bash
openclaw cron add \
  --name "self-evolve" \
  --cron "0 22 * * *" \
  --tz "UTC" \
  --session isolated \
  --message "Load the self-evolve skill and run the daily analysis workflow on yesterday's sessions. Log findings to evolutions/YYYY-MM-DD.md and update evolutions/last-run.json." \
  --timeout-seconds 120 \
  --announce \
  --description "Daily self-evolution: analyze yesterday's sessions"
```

Adjust the cron expression and timezone to your preferred morning time. The example above runs at 22:00 UTC (9:00 AM AEDT).

The `--session isolated` flag ensures it runs in its own session without polluting your main chat. `--announce` delivers a summary to your last active channel when complete.

## Guardrails

- Never auto-delete existing skills or rules — only add or augment
- Flag low-confidence changes for user review instead of auto-applying
- Don't create skills for one-off tasks — require at least 2 occurrences or strong signal
- Keep created skills minimal — follow skill-creator's "concise is key" principle
- Always log before acting — the evolution log is the audit trail
