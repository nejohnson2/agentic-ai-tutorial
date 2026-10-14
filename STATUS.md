# STATUS

**Last updated:** 2026-10-13

## Current state
Complete and verified. All three notebooks execute cleanly; `agent_tutorial.ipynb` runs top to bottom against local Ollama (gemma4:26b) with no errors; outputs are saved in the notebook.

## Completed
- `agent_pydantic_ai.ipynb`: same agent rebuilt with Pydantic AI 2.56 on Ollama (`ollama:gemma4:26b`): tools, transcript inspection, structured output, deps injection, multi-step task with typed result, memory, usage. Executes cleanly.
- `tokens_tutorial.ipynb`: colored token visualizer (tiktoken o200k_base), case/space/number/language/emoji demos, from-scratch BPE trainer, strawberry problem, cost/context, 15-min talk plan. Executes cleanly.
- Seven-section tutorial notebook: single call, memory, tools, manual round-trip, agent loop, multi-step file task, next steps
- README with setup, model comparison table; pinned requirements.txt; .gitignore
- Tested models on the multi-step task: gemma4:26b (good), qwen3:32b (correct, slow), qwen2.5:7b (messy), llama3.1 (wrong)

## Left to do
- Nothing required. Possible extensions are listed in section 7 of the notebook (interactive approval, web/pandas tool, Anthropic API variant).

## Known issues
- `gpt-oss:20b` in the local Ollama library is corrupt ("tensor size overflow"); re-pull it if you want to try it.
- Small models (llama3.1) are unreliable at chaining dependent tool calls; this is documented in section 6.
