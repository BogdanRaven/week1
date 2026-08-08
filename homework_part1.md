# AI Course Homework — Part 1: Model Selection & Setup

## Benchmark task

The same prompt was given to three models:

> Write a Python function called `calculate_fibonacci` that takes an integer n and returns the nth Fibonacci number. Include type hints and a docstring.

## Benchmark setup

- Models: Terra, Luna, and Sol
- Effort: low for all three models
- Prompt: identical for every model
- Fibonacci convention: zero-indexed (`F(0) = 0`, `F(1) = 1`)
- Negative inputs: should be handled explicitly

## Results

| Model | Response time* | Input tokens** | Code output tokens** | Total tokens** |
|---|---:|---:|---:|---:|
| Terra | 6.084 s | 30 | 74 | 104 |
| Luna | 5.442 s | 30 | 91 | 121 |
| Sol | 4.806 s | 30 | 91 | 121 |

\* Response time is the end-to-end sub-agent call time, including startup and scheduling. It is not pure model generation time.

\*\* Token counts are visible-token estimates calculated with the local `o200k_base` tokenizer. Hidden system instructions and internal tool overhead are excluded. The models themselves did not have access to exact usage telemetry.

## Code quality

All three models produced working iterative implementations:

- Correctly calculate zero-indexed Fibonacci numbers.
- Handle `n = 0` and `n = 1` correctly.
- Raise `ValueError` for negative inputs.
- Include the requested type hints and a docstring.
- Use O(n) time and O(1) additional space.

The outputs were very similar. Terra was the most concise in the second run, while Luna and Sol provided slightly more descriptive docstrings. In the initial run, Terra produced the most complete documentation by including argument and exception sections.

## Preferred model

I prefer **Terra** for this task because its initial response included the clearest documentation, including the input argument and possible exception. The difference is small for this simple problem. Sol had the fastest measured end-to-end time in the second run, but those timings include sub-agent overhead.

## Conclusion

At low effort, all three models solved the task correctly. The main difference was documentation detail rather than algorithmic quality. Terra was the preferred model based on the clearest overall explanation, while Sol was fastest in the measured rerun.
