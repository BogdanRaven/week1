# Part 1 — Model Selection & Setup

## Prompt

> Write a Python function called `calculate_fibonacci` that takes an integer n and returns the nth Fibonacci number. Include type hints and a docstring.

## Method

The prompt was run against Terra, Luna, Sol, GPT-5.5, GPT-5.4, and GPT-5.4 Mini at low, medium, and high effort. Outputs were checked for correctness, negative-input handling, type hints, and a docstring.

## Result

All 18 configurations produced correct iterative implementations. Terra had the highest weighted visible-token throughput; GPT-5.5 and GPT-5.4 Mini generated more tokens and had lower measured throughput in this orchestration setup.

See [the detailed Part 1 report](../homework_part1.md) and [the metrics record](../results/part1_metrics.md).
