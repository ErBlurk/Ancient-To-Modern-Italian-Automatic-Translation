# Ancient to Modern Italian Automatic Translation

##### Multilingual Natural Language Processing - Master in Artificial Intelligence and Robotics, Sapienza University of Rome

---

### Authors:
> 1986191: Leonardo Mariut \
> 2202212: Kevin Giannandrea

---

[Original repository here](https://github.com/giankev/Ancient-to-Modern-Italian-Automatic-Translation.git)

[Detailed project report here (PDF)](report.pdf)

---

## Overview

This project arises from the need to benchmark large language models and to test how well they can be used as automatic judges. There is no single established metric for evaluating LLMs on niche tasks like ancient to modern language conversion, so we compare human annotation and LLM-based evaluation side by side. At the same time we show that LLMs can be strong translators, in some cases producing outputs comparable to human experts, while still posing challenges for reliable automatic evaluation.

This study compares three systems: Gemma 2B-it, Minerva350M (fine tuned) and OpusMT.
Translations were rated by human annotators and by two LLM judges: Prometheus Eval 2.0 7B and Gemini 2.0 Flash Lite.
We then measure agreement between human and automatic evaluations.

## Models and approach

* **Gemma 2B-it**: context learning with three example pairs in the prompt.
* **Minerva350M-finetuned**: fine tuned on a task-specific dataset extracted from La Divina Commedia and on LIMA, then used with in-context examples.
* **OpusMT**: pivot strategy. ancient Italian → English → modern Italian using opus-mt-it-en and opus-mt-en-it.

## Datasets

* Main annotation set: 97 ancient Italian sentences with human gold translations.
* Auxiliary: full Divina Commedia split into sentences paired with modern Italian paraphrases for fine tuning.

## Evaluation

* Two dimensions: semantic fidelity and grammatical correctness. Scores 1 (poor) to 5 (excellent).
* For Gemma 2B-it we also tried a secondary rubric focused on fluency and naturalness on a 3-point scale.
* Automatic judges: Prometheus Eval 2.0 7B and Gemini 2.0 Flash Lite.
* Agreement measured with Cohen’s kappa and Spearman correlation.

## Key findings

* Overall agreement between LLM judges and humans is very low.
* OpusMT (pivot) unexpectedly shows the highest agreement with Gemini 2.0.
* Minerva350M needed task-specific fine tuning to produce meaningful outputs but still lags in fluency and coherence.
* Gemma 2B-it yields the smoothest style but can mistranslate when prompts differ from the target structure.
* LLM-as-judge models tend to cluster scores and underuse extreme ratings.

## Agreement scores

| Model                          | Judge                 | Cohen's kappa | Spearman's rho |
| ------------------------------ | --------------------- | ------------: | -------------: |
| gemma2b\_it\_context\_learning | gemini-2.0-flash-lite |        0.0033 |         0.2463 |
| gemma2b\_it\_context\_learning | prometheus            |        -0.047 |         0.0406 |
| opusMT                         | gemini-2.0-flash-lite |        0.0427 |         0.3939 |
| opusMT                         | prometheus            |        0.0985 |          0.079 |
| minerva350M\_finetuned         | gemini-2.0-flash-lite |       -0.0356 |        -0.0459 |
| minerva350M\_finetuned         | prometheus            |        0.0148 |         0.1026 |

## Reproducibility

See `notebooks` in various folders for step by step code and exact prompts.

OpusMT pivot was run on an NVIDIA GeForce 1660 Ti Mobile (6 GB VRAM), batch size 8.


Minerva fine tuning used an RTX 4070 Ti Super (16 GB VRAM), float16, batch size 4, 3 epochs.
