# Compute Budget

This document summarizes the estimated training compute used in our experiments and provides guidance for reproducing the models under different compute budgets.

---

# Training Configuration

## Base Model

- **Model:** Llama-3.2-3B-Instruct
- **Parameters:** ~3.21 Billion

---

## Supervised Fine-Tuning (SFT)

| Hyperparameter | Value |
|----------------|------:|
| Training Examples | 5,000 |
| Epochs | 2 |
| Per-device Batch Size | 4 |
| Gradient Accumulation Steps | 4 |
| Effective Batch Size | 16 |
| Maximum Sequence Length | 2048 |
| Fine-tuning Method | QLoRA |
| Learning Rate | 5e-6 |
| Warmup Ratio | 0.05 |
| Learning Rate Scheduler | Cosine |
| Optimizer | AdamW 8-bit |
| Weight Decay | 0.01 |
| Precision | BF16 |
| Save Total Limit | 2 |

---

## Group Relative Policy Optimization (GRPO)

Each GRPO model is initialized from the trained SFT adapter.

| Hyperparameter | Value |
|----------------|------:|
| Training Examples | 5,000 |
| Epochs | 1 |
| Per-device Batch Size | 4 |
| Gradient Accumulation Steps | 8 |
| Effective Batch Size | 32 |
| Number of Generations | 4 |
| Maximum Prompt Length | 2048 |
| Maximum Completion Length | 32 |
| Learning Rate | 5e-6 |
| Warmup Ratio | 0.05 |
| Learning Rate Scheduler | Cosine |
| Optimizer | AdamW 8-bit |
| Weight Decay | 0.01 |
| Precision | BF16 |
| Save Steps | 25 |
| Save Total Limit | 2 |

Three independent GRPO models are trained:

1. Exact Ranking Reward
2. Relative Ranking Reward
3. Composite Ranking Reward

---

# FLOP Estimation Method

Following the standard dense-equivalent transformer training approximation,

```text
FLOPs ≈ 6 × Nparams × Ntokens
```

where:

- **Nparams** is the number of parameters in the base model.
- **Ntokens** is the total number of tokens processed during training.

For GRPO, the total token count additionally accounts for the four generated completions sampled for each training prompt. The FLOP estimate is reported as a dense-equivalent approximation and does not distinguish between frozen and trainable LoRA parameters.

---

# FLOP Calculation

## Model Parameters

```text
Model Parameters = 3.21 × 10⁹
```

---

## Supervised Fine-Tuning (SFT)

Training examples

```text
5,000
```

Epochs

```text
2
```

Maximum sequence length

```text
2048
```

Total tokens processed

```text
5,000 × 2 × 2048

= 20,480,000 tokens
```

Estimated FLOPs

```text
6 × 3.21 × 10⁹ × 20,480,000

≈ 3.94 × 10¹⁷ FLOPs
```

---

## Group Relative Policy Optimization (GRPO)

Training examples

```text
5,000
```

Epochs

```text
1
```

Generations

```text
4
```

Maximum prompt length

```text
2048
```

Maximum completion length

```text
32
```

Tokens processed per reward model

```text
5,000 × 1 × 4 × (2048 + 32)

= 5,000 × 4 × 2080

= 41,600,000 tokens
```

Estimated FLOPs per reward model

```text
6 × 3.21 × 10⁹ × 41,600,000

≈ 8.01 × 10¹⁷ FLOPs
```

---

## Three GRPO Models

```text
3 × 8.01 × 10¹⁷

≈ 2.40 × 10¹⁸ FLOPs
```

---

# Total Training Compute

```text
SFT FLOPs

3.94 × 10¹⁷

+

Three GRPO Models

2.40 × 10¹⁸

=

2.80 × 10¹⁸ FLOPs
```

---

# Final FLOP Budget

**Estimated dense-equivalent training compute**

```text
≈ 2.8 × 10¹⁸ FLOPs
```

The estimated compute is well below the **1 × 10²⁰ FLOP** budget commonly used for lightweight training tracks.

---

# Compute Budget Considerations

The complete training pipeline consists of:

1. Supervised Fine-Tuning (SFT)
2. GRPO with Exact Ranking Reward
3. GRPO with Relative Ranking Reward
4. GRPO with Composite Ranking Reward

The reported experiments first perform supervised fine-tuning (SFT) and subsequently initialize each of the three GRPO reward models independently from the resulting SFT adapter.


Among the three reward formulations evaluated in this work, the **Composite Ranking Reward (GRPO Model 3)** achieved the strongest overall performance, obtaining the highest Exact Match, Pairwise Agreement, and Macro Precision.

Therefore, if only a single GRPO model is to be reproduced, we recommend training the **Composite Ranking Reward** model after SFT. This provides the best trade-off between computational cost and evaluation performance while closely matching the results reported in the paper.

---

# Reproducibility Note

The FLOP estimates reported above are based on the actual training configuration used in this work, including:

- Llama-3.2-3B-Instruct as the base model.
- 5,000 training examples.
- Two epochs of supervised fine-tuning.
- Three independent GRPO models initialized from the SFT adapter.
- Four generated completions per prompt during GRPO training.

These estimates are intended as dense-equivalent approximations to facilitate reproducibility and comparison across training configurations.