# AI Course Homework — Part 1: Model Selection & Setup

## Benchmark task

The same prompt was given to three models:

> Write a Python function called `calculate_fibonacci` that takes an integer n and returns the nth Fibonacci number. Include type hints and a docstring.

## Benchmark setup

- Models: Terra, Luna, and Sol
- Effort: low, medium, and high
- Prompt: identical for every model
- Fibonacci convention: zero-indexed (`F(0) = 0`, `F(1) = 1`)
- Negative inputs: should be handled explicitly

## Low-effort results

| Model | Response time* | Input tokens** | Code output tokens** | Total tokens** |
|---|---:|---:|---:|---:|
| Terra | 6.084 s | 30 | 74 | 104 |
| Luna | 5.442 s | 30 | 91 | 121 |
| Sol | 4.806 s | 30 | 91 | 121 |

## Medium- and high-effort results

| Model | Effort | Response time* | Input tokens** | Code output tokens** | Total tokens** |
|---|---|---:|---:|---:|---:|
| Terra | Medium | 5.989 s | 30 | 117 | 147 |
| Terra | High | 5.983 s | 30 | 117 | 147 |
| Luna | Medium | 6.587 s | 30 | 91 | 121 |
| Luna | High | 6.591 s | 30 | 91 | 121 |
| Sol | Medium | 6.594 s | 30 | 91 | 121 |
| Sol | High | 6.597 s | 30 | 93 | 123 |

\* Response time is the end-to-end sub-agent call time, including startup and scheduling. It is not pure model generation time.

\*\* Token counts are visible-token estimates calculated with the local `o200k_base` tokenizer. Hidden system instructions and internal tool overhead are excluded. The models themselves did not have access to exact usage telemetry.

## Code quality

All nine configurations produced working iterative implementations:

- Correctly calculate zero-indexed Fibonacci numbers.
- Handle `n = 0` and `n = 1` correctly.
- Raise `ValueError` for negative inputs.
- Include the requested type hints and a docstring.
- Use O(n) time and O(1) additional space.

Terra consistently generated the most detailed docstring, including argument and exception sections at medium and high effort. Luna and Sol used shorter docstrings. Increasing effort did not materially change the algorithm or correctness for this simple task.

The measured times were close within each model. Because they include sub-agent startup and scheduling, they should not be interpreted as pure generation-time measurements.

## Preferred model

I prefer **Terra** for this task because it consistently provided the clearest documentation. Sol had the fastest measured low-effort run, but the timing includes orchestration overhead and does not demonstrate a pure model-speed advantage.

## Conclusion

At low, medium, and high effort, all three models solved the task correctly. The main difference was documentation detail rather than algorithmic quality. For this simple problem, higher effort did not provide a meaningful improvement in correctness or algorithm choice.
