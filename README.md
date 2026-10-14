# Basic Agent Tutorial

Two Jupyter notebooks for learning how LLM agents work, running entirely locally. No API keys, no cost.

- `agent_tutorial.ipynb` — build an agent from scratch on a local model through [Ollama](https://ollama.com)
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

## Tokens tutorial

Uses `tiktoken` (the GPT-4o tokenizer, 200k vocabulary) for the real-tokenizer demos, plus a
25-line Byte Pair Encoding trainer to show how tokenizers are built. Ollama is not needed for this one.
The last cell has a minute-by-minute plan for a 15-minute talk.

```bash
jupyter lab tokens_tutorial.ipynb
```

## Files

- `agent_tutorial.ipynb` — the agent tutorial
- `tokens_tutorial.ipynb` — the tokens tutorial and lecture demos
- `workspace/` — scratch directory used by the agent's file tools
- `requirements.txt` — pinned dependencies
