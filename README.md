# A-MEM Reproducibility Experiment (FTEC5660)

This repository contains a reproducibility experiment of **A-MEM: Agentic Memory for LLM Agents** (Xu et al., NeurIPS 2025) on the LoCoMo benchmark.

Original repository: https://github.com/WujiangXu/A-mem

## What was tested

- Retrieval setting: `retrieve_k` = 0, 5, 10, 15
- Models: `deepseek-chat` and `deepseek-reasoner`
- Dataset size: 10% of LoCoMo (199 questions)

## Requirements

- Python 3.11
- [uv](https://github.com/astral-sh/uv) (recommended) or regular pip
- A DeepSeek API key (OpenAI-compatible endpoint)

## Installation

```bash
# Clone the original repository
git clone https://github.com/WujiangXu/A-mem.git
cd A-mem

# Create virtual environment
uv venv --python 3.11
source .venv/bin/activate   # Windows: .venv\Scripts\activate

# Install dependencies
uv pip install -r requirements.txt
```

## API Configuration

Create a `.env` file in the project root:

```env
OPENAI_API_KEY=your_deepseek_api_key_here
OPENAI_BASE_URL=https://api.deepseek.com/v1
```

## Dataset

The LoCoMo dataset should be placed at:

```
data/locomo10.json
```

(The evaluation script expects this path by default.)

## Running the Evaluation

### Basic command

```bash
uv run python test_advanced_robust.py \
  --backend openai \
  --model deepseek-chat \
  --ratio 0.1 \
  --dataset data/locomo10.json \
  --retrieve_k 5 \
  --output results_k5.json
```

### Examples used in this project

```bash
# deepseek-chat with different retrieve_k
uv run python test_advanced_robust.py --backend openai --model deepseek-chat --ratio 0.1 --dataset data/locomo10.json --retrieve_k 0  --output results_k0.json
uv run python test_advanced_robust.py --backend openai --model deepseek-chat --ratio 0.1 --dataset data/locomo10.json --retrieve_k 5  --output results_k5.json
uv run python test_advanced_robust.py --backend openai --model deepseek-chat --ratio 0.1 --dataset data/locomo10.json --retrieve_k 10 --output results_k10.json
uv run python test_advanced_robust.py --backend openai --model deepseek-chat --ratio 0.1 --dataset data/locomo10.json --retrieve_k 15 --output results_k15.json

# deepseek-reasoner
uv run python test_advanced_robust.py --backend openai --model deepseek-reasoner --ratio 0.1 --dataset data/locomo10.json --retrieve_k 0  --output results_reasoner_k0.json
uv run python test_advanced_robust.py --backend openai --model deepseek-reasoner --ratio 0.1 --dataset data/locomo10.json --retrieve_k 5  --output results_reasoner_k5.json
uv run python test_advanced_robust.py --backend openai --model deepseek-reasoner --ratio 0.1 --dataset data/locomo10.json --retrieve_k 10 --output results_reasoner_k10.json
uv run python test_advanced_robust.py --backend openai --model deepseek-reasoner --ratio 0.1 --dataset data/locomo10.json --retrieve_k 15 --output results_reasoner_k15.json
```

## Notes on Runtime

- **First run** with a new model builds the memory notes using the LLM. This takes a noticeable amount of time.
- **Subsequent runs** with the same model are much faster because the memory notes are cached locally.
- Using `--ratio 0.1` keeps the evaluation size manageable while still providing meaningful results.

## Results

The output JSON files contain aggregate metrics (F1, Exact Match, ROUGE, BERTScore, Adversarial Correct, etc.) under `aggregate_metrics.overall`.

