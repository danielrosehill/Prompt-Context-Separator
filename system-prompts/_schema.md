# Output schema

All system prompts in this directory share the same structured output contract. The agent must return a single JSON object with exactly these fields:

```json
{
  "prompts": ["string", "..."],
  "context": ["string", "..."]
}
```

Field rules:

- **`prompts`** — array of strings. Each item is one self-contained ask the user is putting to the AI. Light cleanup is allowed; no paraphrasing or summarisation. Empty array if the input contains no asks.
- **`context`** — array of strings. Each item is one third-person background chunk, prefixed with `{{user}}`. One chunk per discrete idea. Empty array if the input has no background.

Greetings, sign-offs, and pure filler must be dropped.
