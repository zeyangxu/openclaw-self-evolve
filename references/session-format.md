# OpenClaw Session JSONL Format

Session transcripts live in: `~/.openclaw/agents/main/sessions/`

## File Types
- `<uuid>.jsonl` — active session
- `<uuid>.jsonl.reset.<timestamp>` — archived (reset) session
- `<uuid>.jsonl.lock` — lock file (ignore)
- `sessions.json` — session index

## JSONL Record Types

Each line is a JSON object with a `type` field:

### `type: "session"` — Session metadata (first line)
```json
{"type": "session", "version": 3, "id": "<uuid>", "timestamp": "...", "cwd": "..."}
```

### `type: "message"` — Conversation message
```json
{
  "type": "message",
  "id": "...",
  "parentId": "...",
  "timestamp": "...",
  "message": {
    "role": "user|assistant|tool",
    "content": [
      {"type": "text", "text": "..."},
      {"type": "toolCall", "id": "...", "name": "...", "arguments": {...}},
      {"type": "toolResult", "toolCallId": "...", "content": "..."}
    ]
  }
}
```

### Other types (skip during analysis)
- `type: "thinking_level_change"`
- `type: "custom"` (model snapshots, etc.)

## Parsing Pattern

```python
import json

with open(session_file) as f:
    for line in f:
        d = json.loads(line.strip())
        if d.get("type") != "message":
            continue
        msg = d["message"]
        role = msg["role"]
        content = msg.get("content", "")
        if isinstance(content, list):
            texts = [c["text"] for c in content if c.get("type") == "text"]
            content = " ".join(texts)
        # Now `role` and `content` are usable
```
