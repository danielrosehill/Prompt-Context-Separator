# Prompt-Context-Separator

A utility for cleanly separating prompt instructions from contextual material in voice-typed / dictated LLM input.

## Why

Long dictated prompts blur three distinct things:

1. **Prompts** — the discrete asks. What the model should actually answer or do.
2. **Context** — background that grounds the asks but is not itself a question.
3. **Host notes** — meta-instructions about tone, format, or persona.

Separating them makes prompts reusable across different context, makes context reusable across different asks, and makes models follow the instructions more accurately.

## Schema

This repo models the structure of the [`danielrosehill/Prompt-Separation`](https://huggingface.co/datasets/danielrosehill/Prompt-Separation) dataset on Hugging Face — a wide flat schema where prompts and context chunks each get their own column.

| column | type | description |
|---|---|---|
| `episode_id` | int | Stable primary key from the upstream MWP DB. |
| `prompt_transcript` | string | Voice-typed transcript as originally received. |
| `source` | string | `human` (gold) or `ai_extrapolation` (silver). |
| `silver_model` | string | Model name for AI rows. Empty for human rows. |
| `labelled_at` | string | ISO date the label was produced. |
| `n_prompts` | int | Number of populated `promptN` slots. |
| `n_context` | int | Number of populated `contextN` slots. |
| `prompt1` … `prompt10` | string | One discrete ask per slot. Light cleanup, no paraphrasing. Empty for unused slots. |
| `context1` … `context20` | string | One context chunk per slot. Third-person, prefixed `{{user}}`. Empty for unused slots. |
| `host_notes` | string | Direct meta-instructions to the host (tone, format, persona). |

Maximum slot counts (10 prompts, 20 context chunks) are determined by the actual data in the upstream corpus.

## Separation rules

> A **prompt** is a specific question or task — what the author is actually asking the AI to do or answer. Each distinct ask is its own prompt.
>
> **Context** is background that grounds the prompts but is not itself an ask — prior thinking, motivation, anecdotes, framing. Returned as a list of chunks, third-person, prefixed `{{user}}`.
>
> **host_notes** are direct instructions to the AI host about tone, focus, format, or persona.
>
> Greetings, sign-offs, and pure filler are dropped. Light transcription cleanup is allowed; no paraphrasing or summarisation.

## Gold sample

`data/gold.csv` and `data/gold.jsonl` contain the 10 human-annotated rows (`source = human`) from the upstream dataset, included here as canonical examples of correct separation. The JSONL form collapses the wide `promptN` / `contextN` slots into `prompts` and `context` lists for easier programmatic use.

## Related

- Dataset: <https://huggingface.co/datasets/danielrosehill/Prompt-Separation>
- Upstream pipeline / labelling code: <https://github.com/danielrosehill/MWP-Prompts-0426>
- Source corpus: [My Weird Prompts](https://myweirdprompts.com)

## License

MIT (code) — gold rows derived from the upstream dataset retain its CC-BY-4.0 license.
