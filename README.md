# Prompt-Context-Separator

A utility for cleanly separating prompt instructions from contextual material when working with LLMs.

## Why

Long prompts often blur two distinct things:

1. **Instructions** — what the model should do.
2. **Context** — the material it should do it against (documents, transcripts, data, prior outputs).

Mixing them makes prompts harder to maintain, harder to reuse across contexts, and harder for models to follow accurately.

## What this does

Takes a single mixed input and splits it into two clearly delimited sections:

- `## Instructions`
- `## Context`

So the same instruction set can be reused with different context, and the same context can be re-run against different instructions.

## Status

Early scaffold — implementation TBD.

## License

MIT
