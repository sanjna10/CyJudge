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
| Effective Batch Size | 16 |
| Maximum Sequence Length | 2048 |
| Fine-tuning Method | QLoRA |

---

## GRPO

Each GRPO model is initialized from the trained SFT adapter.

| Hyperparameter | Value |
|----------------|------:|
| Training Examples | 5,000 |
| Epochs | 1 |
| Effective Batch Size | 32 |
| Number of Generations | 4 |
| Maximum Prompt Length | 2048 |
| Maximum Completion Length | 32 |

Three independent GRPO models are trained:

1. Exact Ranking Reward
2. Relative Ranking Reward
3. Composite Ranking Reward

---

# FLOP Estimation Method

Following the standard dense-equivalent transformer training approximation,

\[
\boxed{\text{FLOPs} \approx 6 \times N_{\text{params}} \times N_{\text{tokens}}}
\]

where

- \(N_{\text{params}}\) is the number of model parameters.
- \(N_{\text{tokens}}\) is the total number of tokens processed during training.

For GRPO, the total token count additionally includes multiple generated completions per prompt.

---

# FLOP Calculation

## Parameters

```
Model Parameters = 3.21 × 10⁹
```

---

## SFT

Training examples

```
5000
```

Epochs

```
2
```

Maximum sequence length

```
2048
```

Total tokens processed

```
5000 × 2 × 2048
= 20,480,000 tokens
```

Estimated FLOPs

```
6 × 3.21 × 10⁹ × 20,480,000
≈ 3.94 × 10¹⁷ FLOPs
```

---

## GRPO

Training examples

```
5000
```

Epochs

```
1
```

Generations

```
4
```

Prompt length

```
2048
```

Completion length

```
32
```

Tokens processed per reward model

```
5000 × 1 × 4 × (2048 + 32)

= 5000 × 4 × 2080

= 41,600,000 tokens
```

Estimated FLOPs per reward model

```
6 × 3.21 × 10⁹ × 41,600,000

≈ 8.01 × 10¹⁷ FLOPs
```

---

## Three GRPO Models

```
3 × 8.01 × 10¹⁷

≈ 2.40 × 10¹⁸ FLOPs
```

---

# Total Training Compute

```
SFT FLOPs

3.94 × 10¹⁷

+

Three GRPO models

2.40 × 10¹⁸

=

2.80 × 10¹⁸ FLOPs
```

---

# Final FLOP Budget

**Estimated dense-equivalent training compute**

```
≈ 2.8 × 10¹⁸ FLOPs
```

This is well below the **1 × 10²⁰ FLOP** budget commonly used for lightweight training tracks.

---

# Compute Budget Considerations

The complete training pipeline consists of:

1. Supervised Fine-Tuning (SFT)
2. GRPO with Exact Ranking Reward
3. GRPO with Relative Ranking Reward
4. GRPO with Composite Ranking Reward

The reported experiments train all three GRPO reward models independently using the same SFT adapter as initialization.

If compute or storage constraints prevent reproducing all reward models, users may choose to train **only one** GRPO model after completing SFT. This substantially reduces the overall training cost while still reproducing one of the proposed reward formulations.

Among the three reward formulations evaluated in this work, the **Composite Ranking Reward (GRPO Model 3)** achieved the strongest overall performance, obtaining the highest Exact Match, Pairwise Agreement, and Macro Precision.

Therefore, if only a single GRPO model is to be reproduced, we recommend training the **Composite Ranking Reward** model after SFT. This provides the best trade-off between computational cost and evaluation performance while closely matching the results reported in the paper.