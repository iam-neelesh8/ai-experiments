# ai-experiments

A scratchpad for AI/ML experiments — models, prompts, agents, retrieval, evaluation.

## Layout

Nothing is enforced here. A loose convention that keeps things findable:

```
experiments/     one folder per experiment, dated or named
notebooks/       exploratory notebooks
data/            local data (gitignored)
```

## Convention

Each experiment gets its own folder with a short `README.md` answering three
questions: what was tried, what happened, and whether it is worth pursuing.
Negative results are worth keeping — they are the cheapest thing in the repo
and the easiest to forget.

## Setup

```bash
python -m venv .venv
.venv\Scripts\activate      # Windows
pip install -r requirements.txt
```

Keep API keys in a `.env` file, which is gitignored.
