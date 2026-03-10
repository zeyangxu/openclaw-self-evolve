# openclaw-self-evolve

A self-evolving skill for [OpenClaw](https://github.com/openclaw/openclaw) agents. Analyzes yesterday's session histories each morning to discover improvement opportunities — then automatically creates new skills, references, or rules.

## What It Does

- Reviews prior session transcripts for patterns, gaps, and failures
- Identifies repeated struggles, missing context, unsatisfied answers
- Generates new skills, references, or workspace rules automatically
- Logs every evolution with rationale

## Install

```bash
# Clone into your OpenClaw skills directory
git clone git@github.com:zeyangxu/openclaw-self-evolve.git ~/.openclaw/workspace/skills/self-evolve
```

Or add to your `HEARTBEAT.md` for automatic daily runs:

```markdown
## Morning Self-Evolve (run once per day, before 10:00 UTC)
Check `evolutions/last-run.json` for last run date. If it's not today:
1. Load the `self-evolve` skill
2. Run the daily analysis workflow on yesterday's sessions
3. Log findings to `evolutions/YYYY-MM-DD.md`
4. Update `evolutions/last-run.json` with today's date
```

## Structure

```
self-evolve/
├── SKILL.md                          # Main skill instructions
└── references/
    ├── analysis-framework.md         # How to analyze sessions
    └── session-format.md             # Session log format reference
```

## License

MIT
