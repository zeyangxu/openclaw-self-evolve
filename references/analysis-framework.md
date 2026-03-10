# Session Analysis Framework

## Signal Detection Rubric

### 1. Repeated Patterns (High Value)
- Same tool sequence used 3+ times across sessions
- Same question phrased differently in multiple sessions
- Same type of document/data requested repeatedly
- **Threshold**: 2+ occurrences = candidate for skill/script

### 2. Unsatisfied Answers (High Value)
- User says: "no", "that's wrong", "not what I meant", "try again"
- User rephrases the same request
- User manually corrects the output
- Conversation goes >3 turns on what should be simple
- **Threshold**: Any occurrence = investigate root cause

### 3. Missing Context (Medium Value)
- Agent asks "what is X?" where X is a project/tool/person the user works with daily
- Agent makes wrong assumptions about environment, repos, or workflows
- User has to re-explain their setup or preferences
- **Threshold**: Any occurrence = update relevant .md file

### 4. Skill Gaps (High Value)
- Agent says "I don't have a skill for this" or improvises a multi-step workflow
- Agent produces working but verbose/fragile solutions
- Task requires domain knowledge the agent lacks
- **Threshold**: 2+ occurrences or 1 complex occurrence = create skill

### 5. Tool Failures (Medium Value)
- API errors, permission issues, wrong parameters
- Agent retries the same tool call multiple times
- Agent falls back to manual/alternative approach
- **Threshold**: 2+ occurrences of same failure = add troubleshooting to skill or reference

### 6. Slow Workflows (Low-Medium Value)
- 5+ sequential tool calls for what could be one script
- Agent generates code inline that could be a reusable script
- Back-and-forth that could be eliminated with better defaults
- **Threshold**: 3+ occurrences or significant time waste = script it

## Analysis Output Template

For each session analyzed, produce:

```
### Session: [session_key]
- **Topic**: [brief description]
- **Turns**: [count]
- **Signals found**: [list of signal types]
- **Details**: [specific findings]
- **Recommended action**: [what to create/update]
```

## Priority Matrix

| Signal Type | Frequency | Action |
|---|---|---|
| Repeated pattern | 2+ times | Create skill or script |
| Unsatisfied answer | Any | Investigate, fix root cause |
| Missing context | Any | Update .md files immediately |
| Skill gap | 2+ or complex | Create skill |
| Tool failure | 2+ same | Add to troubleshooting reference |
| Slow workflow | 3+ or obvious | Create script |

## What NOT to Create

- Skills for one-off tasks the user will never repeat
- Rules that contradict existing SOUL.md personality
- References for publicly available documentation (use web_search instead)
- Overly specific skills that won't generalize
