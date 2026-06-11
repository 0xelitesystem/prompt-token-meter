# prompt-token-meter

Visual context-window meter. Paste a prompt, see estimated token usage as gauges across major LLMs (Claude, GPT, Gemini, Llama, DeepSeek, Mistral). Browser only.

**Live demo:** https://0xelitesystem.github.io/prompt-token-meter/

## Use

Open [`index.html`](./index.html). Paste a prompt. Live counter updates as you type. Click Measure to populate the gauge cluster.

Each gauge shows:

- The model and its context window size
- Percentage used (gauge fill + readout)
- Tokens used and remaining
- Status indicator (green / amber / red)

When you exceed a model's context, the gauge goes red and reports how many tokens you're over.

## Why

Knowing roughly how many tokens your prompt will consume, and how that fits within different models' context budgets, is useful for:

- Choosing which model to send a long prompt to
- Trimming a prompt before it hits an API limit
- Comparing context-budget-per-dollar across models
- Understanding why your "agentic loop" is starting to drop earlier instructions

The gauges make this comparison spatial. You see at a glance which models will fit, which are tight, and which won't take it.

## Heuristic, not exact

This uses a heuristic estimate (roughly 4 chars per token for English prose, adjusted for code density). Real tokenizers vary by model:

- Claude uses BPE
- GPT uses tiktoken
- Gemini uses SentencePiece
- Llama uses its own tokenizer

The estimate is generally within ±15% for English text. For non-English (especially CJK), it under-counts significantly. For pure code, it's closer.

If you need exact numbers, use the model's official tokenizer (Anthropic's `count_tokens` API, tiktoken, etc.). This tool is for "is my prompt 5K or 50K tokens" planning, not billing-precise counts.

## What's not in the meter

- Image and document attachments (they consume tokens too, but this tool only measures text)
- Output budget (the context is shared between input and output, if you ask for a long reply, your input budget is correspondingly smaller)
- System prompt overhead (varies by tool, Claude Code, Cursor, etc. all add their own context)

## Models included

The list reflects current widely-available models. Update as they ship:

- Claude 4.7 family (Opus, Sonnet, Haiku), 200K context
- GPT-5 to 256K
- GPT-4o, 128K
- Gemini 2.5 Pro / Flash, 1M
- Llama 3.3 70B, 128K
- DeepSeek V3 to 128K
- Mistral Large 2 to 128K

## Privacy

All token estimation runs in your browser. The prompt you paste does not leave your device.

## Run locally

```
git clone https://github.com/0xelitesystem/prompt-token-meter
cd prompt-token-meter
```

Open `index.html` in a browser.

## Contribute

PRs welcome:

- Better heuristics (especially for CJK, code, structured data)
- Optional integration with tokenizers via WASM (for accuracy when network is OK)
- Per-model heuristic correction factors based on benchmarks
- New models as they ship

Don't add: API calls, telemetry, npm dependencies. Single file.

## Build

No build. Single HTML file.

## License

MIT.

## Related

- [prompt-cost-calculator](https://github.com/0xelitesystem/prompt-cost-calculator) - cost estimation across models
- [llm-pricing-data](https://github.com/0xelitesystem/llm-pricing-data) - structured data on model pricing
- [agentic-workflow-patterns](https://github.com/0xelitesystem/agentic-workflow-patterns) - reset-the-context pattern
