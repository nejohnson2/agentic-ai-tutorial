# Basic Agent Tutorial

Three Jupyter notebooks for learning how LLM agents work, running entirely locally. No API keys, no cost.

- `agent_tutorial.ipynb` — build an agent from scratch on a local model through [Ollama](https://ollama.com)
- `agent_pydantic_ai.ipynb` — the same agent rebuilt with the Pydantic AI framework: typed outputs, dependencies, memory
- `tokens_tutorial.ipynb` — what tokens are, a BPE tokenizer built from scratch, and demos for a 15-minute lecture

## Agent tutorial

The notebook builds an agent up one piece at a time:

1. A single LLM call
2. Conversation memory (a list of messages)
3. Tools as plain Python functions
4. One manual tool round-trip
5. The agent loop (`run_agent`)
6. A multi-step task that chains file reads, a calculator, and file writes
7. Where to go next

## Setup

```bash
# 1. Ollama must be installed and running, with a tool-capable model pulled
ollama pull gemma4:26b          # default in the notebook; qwen2.5:7b is a smaller fallback

# 2. Python environment
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# 3. Open the notebook
jupyter lab agent_tutorial.ipynb
```

Run the cells top to bottom. The `workspace/` folder is where the agent's file tools read and write.

## Model choice

Any Ollama model with tool support works for sections 1 to 5. Section 6 requires the model to chain
dependent tool calls, and small models struggle there. Observed on this machine (Apple Silicon):

| Model | Section 6 result |
|---|---|
| gemma4:26b | Correct, 4 clean steps, about 17 s |
| qwen3:32b | Correct, but about 3 min (thinking model) |
| qwen2.5:7b | Correct after several failed calculator calls |
| llama3.1 (8B) | Wrong: called every tool at once and hallucinated file contents |

Change `MODEL` in the setup cell to experiment.

## Pydantic AI tutorial

Rebuilds the hand-written agent with [Pydantic AI](https://ai.pydantic.dev), pointing out what the
framework does at each step: the loop runs inside `agent.run`, tools are `@agent.tool` decorators,
final answers are validated Pydantic models (`output_type=`), shared state is injected via `deps_type`,
and memory is `message_history=`. Ends with the same multi-step expenses task returning a typed result.

Pydantic AI reaches Ollama through its OpenAI-compatible endpoint; the notebook sets `OLLAMA_BASE_URL`
itself. Uses `await` at the top level of cells, which Jupyter supports natively.

```bash
jupyter lab agent_pydantic_ai.ipynb
```

## Tokens tutorial

Uses `tiktoken` (the GPT-4o tokenizer, 200k vocabulary) for the real-tokenizer demos, plus a
25-line Byte Pair Encoding trainer to show how tokenizers are built. Ollama is not needed for this one.
The last cell has a minute-by-minute plan for a 15-minute talk.

```bash
jupyter lab tokens_tutorial.ipynb
```

## Files

- `agent_tutorial.ipynb` — the agent tutorial, by hand
- `agent_pydantic_ai.ipynb` — the agent tutorial, with Pydantic AI
- `tokens_tutorial.ipynb` — the tokens tutorial and lecture demos
- `workspace/` — scratch directory used by the agent's file tools
- `requirements.txt` — pinned dependencies
