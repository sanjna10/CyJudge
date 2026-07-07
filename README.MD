# CyJudge: Finetuned LLM-as-a-Judge for Cybersecurity Open-Ended Question Answering

This repository contains the code, trained adapters, inference notebooks, evaluation scripts, and prediction outputs used for the paper **"Understanding Small Scale Fine-tuning of LLM-as-a-Judge: A Case Study in Cybersecurity."**

---

---

> [!IMPORTANT]
> **Please use the updated notebooks included in this repository for reproducing the experiments.**
>
> - `Inference.ipynb`
> - `inference_metrics.ipynb`
> - `sft_training.ipynb`
> - `grpo_base_models.ipynb`
>
> These notebooks contain the **final training, inference, and evaluation pipeline**, including the updated  explicit step-by-step instructions, required package installation commands, dependencies, paths. They should be used as the primary reference for reproducing the results reported in the paper.

# Additional Resources

The complete training checkpoints, datasets, inference notebooks, evaluation scripts, and prediction outputs used in this work are available in the anonymous artifact repository:

**https://anonymous.4open.science/r/CyJudge-C138/README.MD**

The released artifacts are sufficient to reproduce the reported experiments and evaluation results.
Flop budget is calculated in `flop_budget.md`
dependencies are explicitly mentioned on the first cell of every `.ipynb` notebook and on `dependencies_imports.ipynb`
---

# Project Structure

```
colm_grpo_llmaj_dataset/
│
├── train/
│   └── llama3b/
│       ├── sft_adapter/
│       ├── sft_outputs/
│       ├── exact_ranking_reward_adapter/
│       ├── relative_ranking_reward_adapter/
│       └── composite_ranking_reward_adapter/
│
├── test/
│   └── llama3b/
│       ├── exact_ranking_reward_run1.csv
│       ├── exact_ranking_reward_run2.csv
│       ├── relative_ranking_reward_run1.csv
│       ├── relative_ranking_reward_run2.csv
│       ├── composite_ranking_reward_run1.csv
│       └── composite_ranking_reward_run2.csv
│
├── train_subset.csv
├── test_subset.csv
├── sft_training.ipynb
├── grpo_base_models.ipynb
├── Inference.ipynb
├── inference_metrics.ipynb
└── README.md
```

---

# Checkpoint Storage

During training, the number of saved checkpoints was limited to the **last two checkpoints** (`save_total_limit = 2`) due to the storage quota available. Earlier checkpoints were automatically removed as training progressed to conserve disk space.

The repository includes the final trained adapters used for all reported experiments, and these are sufficient to reproduce the inference and evaluation results presented in the paper.

---

# Dataset Paths

Training Dataset

```python
TRAIN_CSV = "/content/drive/MyDrive/colm_grpo_llmaj_dataset/train_subset.csv"
```

Test Dataset

```python
TEST_CSV = "/content/drive/MyDrive/colm_grpo_llmaj_dataset/test_subset.csv"
```

---

# Training Adapters

## SFT Adapter

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/train/llama3b/sft_adapter
```

Training outputs

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/train/llama3b/sft_outputs
```

---

## GRPO Model 1 (Exact Ranking Reward)

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/train/llama3b/exact_ranking_reward_adapter
```

---

## GRPO Model 2 (Relative Ranking Reward)

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/train/llama3b/relative_ranking_reward_adapter
```

---

## GRPO Model 3 (Composite Ranking Reward)

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/train/llama3b/composite_ranking_reward_adapter
```

---

# Inference Outputs

## Exact Ranking Reward

Run 1

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/test/llama3b/exact_ranking_reward_run1.csv
```

Run 2

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/test/llama3b/exact_ranking_reward_run2.csv
```

---

## Relative Ranking Reward

Run 1

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/test/llama3b/relative_ranking_reward_run1.csv
```

Run 2

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/test/llama3b/relative_ranking_reward_run2.csv
```

---

## Composite Ranking Reward

Run 1

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/test/llama3b/composite_ranking_reward_run1.csv
```

Run 2

```python
/content/drive/MyDrive/colm_grpo_llmaj_dataset/test/llama3b/composite_ranking_reward_run2.csv
```

---

# Utility Notebooks

SFT Training

```text
/content/drive/MyDrive/colm_grpo_llmaj_dataset/sft_training.ipynb
```

GRPO Training

```text
/content/drive/MyDrive/colm_grpo_llmaj_dataset/grpo_base_models.ipynb
```

Inference

```text
/content/drive/MyDrive/colm_grpo_llmaj_dataset/Inference.ipynb
```

Inference Metrics

```text
/content/drive/MyDrive/colm_grpo_llmaj_dataset/inference_metrics.ipynb
```

---

# Running Inference

The repository already contains all trained adapters.

To generate inference for any model:

1. Open **Inference.ipynb**.
2. Update the adapter path.

Example:

```python
ADAPTER_PATH = "/content/drive/MyDrive/colm_grpo_llmaj_dataset/train/llama3b/sft_adapter"
```

or

```python
ADAPTER_PATH = "/content/drive/MyDrive/colm_grpo_llmaj_dataset/train/llama3b/exact_ranking_reward_adapter"
```

or

```python
ADAPTER_PATH = "/content/drive/MyDrive/colm_grpo_llmaj_dataset/train/llama3b/relative_ranking_reward_adapter"
```

or

```python
ADAPTER_PATH = "/content/drive/MyDrive/colm_grpo_llmaj_dataset/train/llama3b/composite_ranking_reward_adapter"
```

3. Change the output CSV path if desired.

```python
OUTPUT_CSV = "/content/drive/MyDrive/colm_grpo_llmaj_dataset/test/llama3b/model_predictions.csv"
```

4. Run the notebook.

---

# Computing Evaluation Metrics

To compute evaluation metrics:

1. Open **inference_metrics.ipynb**.
2. Change the prediction CSV path to the generated inference file.
3. Run the notebook.

The notebook computes:

- Parse Rate
- Exact Match
- Top-1 Accuracy
- Pairwise Agreement
- Macro Precision
- Macro Recall
- Macro F1

The same notebook can be used for any prediction file by simply changing the CSV path.

---

# Reproducing Training

The repository includes **reference (mock) training notebooks** demonstrating the training pipeline for both SFT and GRPO.

These notebooks illustrate the methodology used in the paper. Users can reproduce the experiments using the hyperparameters below or adapt the notebooks for their own datasets.

---

# Supervised Fine-Tuning (SFT)

Base Model

```
Llama-3.2-3B-Instruct
```

Training Method

```
QLoRA
```

Hyperparameters

| Hyperparameter | Value |
|----------------|------:|
| LoRA Rank | 16 |
| LoRA Alpha | 16 |
| LoRA Dropout | 0 |
| Target Modules | q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj |
| Max Sequence Length | 2048 |
| Epochs | 2 |
| Learning Rate | 5e-6 |
| Effective Batch Size | 16 |
| Optimizer | AdamW 8-bit |
| LR Scheduler | Cosine |
| Warmup Ratio | 0.05 |
| Weight Decay | 0.01 |
| Precision | BF16 |
| Save Steps | 250 |
| Save Total Limit | 2 |

---

# Group Relative Policy Optimization (GRPO)

GRPO training is performed **starting from the SFT adapter**.

Hyperparameters

| Hyperparameter | Value |
|----------------|------:|
| Initialization | SFT Adapter |
| Epochs | 1 |
| Learning Rate | 5e-6 |
| Effective Batch Size | 32 |
| Number of Generations | 4 |
| Max Prompt Length | 2048 |
| Max Completion Length | 32 |
| Optimizer | AdamW 8-bit |
| LR Scheduler | Cosine |
| Warmup Ratio | 0.05 |
| Weight Decay | 0.01 |
| Temperature | 0.7 |
| Top-p | 0.9 |

---

# Reward Functions

Three GRPO reward functions are implemented.

---

## Model 1 — Exact Ranking Reward

The model receives

- +1 for an exact ranking prediction.
- −1 otherwise.
- +0.5 formatting reward if the generated output is a valid ranking.

Overall reward

```
Reward = Exact Match Reward + Formatting Reward
```

---

## Model 2 — Relative Ranking Reward

The reward is computed from the proportion of correctly ordered response pairs.

```
Pairwise Reward = 2 × (Correct Pairs / Total Pairs) − 1
```

A formatting reward of **+0.5** is added for valid rankings.

Overall reward

```
Reward = Pairwise Reward + Formatting Reward
```

---

## Model 3 — Composite Ranking Reward

The reward provides graded feedback based on positional agreement.

Position weights

| Position | Weight |
|----------|-------:|
| First | 0.5 |
| Second | 0.3 |
| Third | 0.2 |

Weighted positional agreement

```
Srank = Σ wi δi
```

where

- δᵢ = 1 if the response at position *i* matches the ground-truth ranking.
- δᵢ = 0 otherwise.

Ranking reward

```
Ranking Reward = 2 × Srank − 1
```

A formatting reward of **+0.5** is added for valid rankings.

Overall reward

```
Reward = Ranking Reward + Formatting Reward
```

---

# Exact Reward Function Code

```python
import re

VALID_RANKINGS = [
    "R1>R2>R3",
    "R1>R3>R2",
    "R2>R1>R3",
    "R2>R3>R1",
    "R3>R1>R2",
    "R3>R2>R1",
]

RANKING_RE = re.compile(r"R[123]\s*>\s*R[123]\s*>\s*R[123]")


def normalize_ranking(text):
    if text is None:
        return None

    match = RANKING_RE.search(str(text).strip())
    if not match:
        return None

    ranking = match.group(0).replace(" ", "")
    return ranking if ranking in VALID_RANKINGS else None


def get_text(completion):
    if isinstance(completion, str):
        return completion

    if isinstance(completion, list):
        if len(completion) > 0 and isinstance(completion[0], dict):
            return completion[0].get("content", "")
        return str(completion[0])

    return str(completion)


def format_reward(pred):
    return 0.5 if pred is not None else 0.0
```

---

## GRPO Model 1: Exact Ranking Reward

```python
def exact_ranking_reward(completions, gold_ranking, **kwargs):
    rewards = []

    for completion, gold in zip(completions, gold_ranking):
        pred = normalize_ranking(get_text(completion))

        correctness = 1.0 if pred == gold else -1.0
        fmt = format_reward(pred)

        rewards.append(correctness + fmt)

    return rewards
```

---

## GRPO Model 2: Relative Ranking Reward

```python
def ranking_to_pairs(ranking):
    if ranking is None:
        return set()

    items = ranking.split(">")
    pairs = set()

    for i in range(len(items)):
        for j in range(i + 1, len(items)):
            pairs.add((items[i], items[j]))

    return pairs


def pairwise_agreement(pred, gold):
    pred_pairs = ranking_to_pairs(pred)
    gold_pairs = ranking_to_pairs(gold)

    if len(pred_pairs) != 3 or len(gold_pairs) != 3:
        return 0.0

    return len(pred_pairs.intersection(gold_pairs)) / 3.0


def relative_ranking_reward(completions, gold_ranking, **kwargs):
    rewards = []

    for completion, gold in zip(completions, gold_ranking):
        pred = normalize_ranking(get_text(completion))

        if pred is None:
            pairwise = -1.0
        else:
            score = pairwise_agreement(pred, gold)
            pairwise = (2 * score) - 1

        fmt = format_reward(pred)
        rewards.append(pairwise + fmt)

    return rewards
```

---

## GRPO Model 3: Composite Ranking Reward

```python
def composite_ranking_reward(completions, gold_ranking, **kwargs):
    rewards = []
    weights = [0.5, 0.3, 0.2]

    for completion, gold in zip(completions, gold_ranking):
        pred = normalize_ranking(get_text(completion))

        if pred is None:
            ranking_reward = -1.0
        else:
            pred_items = pred.split(">")
            gold_items = gold.split(">")

            s_rank = sum(
                w for w, p, g in zip(weights, pred_items, gold_items)
                if p == g
            )

            ranking_reward = (2 * s_rank) - 1

        fmt = format_reward(pred)
        rewards.append(ranking_reward + fmt)

    return rewards
```

# Notes

- The repository already contains the trained SFT adapter.
- The repository already contains all three GRPO-trained adapters.
- The repository also contains prediction outputs from two independent inference runs for each GRPO model.
- The provided notebooks are reference implementations demonstrating the training and evaluation pipeline.
- To evaluate any model, simply update the adapter path in **Inference.ipynb** and the prediction CSV path in **inference_metrics.ipynb**. To run the inference twice you need to seperately run the models twice.
- The released adapters can be used directly for inference without retraining.