# AI Course Homework — Part 1: Model Selection & Setup

## Benchmark task

The same prompt was given to every model:

> Write a Python function called `calculate_fibonacci` that takes an integer n and returns the nth Fibonacci number. Include type hints and a docstring.

## Benchmark setup

- Models: Terra, Luna, Sol, GPT-5.5, GPT-5.4, and GPT-5.4 Mini
- Effort: low, medium, and high
- Prompt: identical for every model
- Fibonacci convention: zero-indexed (`F(0) = 0`, `F(1) = 1`)
- Negative inputs: should be handled explicitly

## Terra, Luna, and Sol results

| Model | Effort | Response time* | Input tokens** | Output tokens** | Total tokens** |
|---|---|---:|---:|---:|---:|
| Terra | Low | 6.084 s | 30 | 74 | 104 |
| Luna | Low | 5.442 s | 30 | 91 | 121 |
| Sol | Low | 4.806 s | 30 | 91 | 121 |
| Terra | Medium | 5.989 s | 30 | 117 | 147 |
| Terra | High | 5.983 s | 30 | 117 | 147 |
| Luna | Medium | 6.587 s | 30 | 91 | 121 |
| Luna | High | 6.591 s | 30 | 91 | 121 |
| Sol | Medium | 6.594 s | 30 | 91 | 121 |
| Sol | High | 6.597 s | 30 | 93 | 123 |

## GPT-5.5, GPT-5.4, and GPT-5.4 Mini results

| Model | Effort | Response time* | Input tokens** | Output tokens** | Total tokens** |
|---|---|---:|---:|---:|---:|
| GPT-5.5 | Low | 9.057 s | 30 | 132 | 162 |
| GPT-5.5 | Medium | 19.126 s | 30 | 151 | 181 |
| GPT-5.5 | High | 19.132 s | 30 | 164 | 194 |
| GPT-5.4 | Low | 4.891 s | 30 | 111 | 141 |
| GPT-5.4 | Medium | 7.106 s | 30 | 111 | 141 |
| GPT-5.4 | High | 19.137 s | 30 | 178 | 208 |
| GPT-5.4 Mini | Low | 19.141 s | 30 | 216 | 246 |
| GPT-5.4 Mini | Medium | 7.098 s | 30 | 202 | 232 |
| GPT-5.4 Mini | High | 19.150 s | 30 | 253 | 283 |

GPT-5.4 Mini included additional optional suggestions outside the code block. Its code-only estimates were 183, 183, and 201 tokens for low, medium, and high effort respectively; the table counts the complete model responses.

\* Response time is the end-to-end sub-agent call time, including startup, scheduling, and concurrency effects. It is not pure model generation time, so small differences should not be treated as definitive speed rankings.

\*\* Token counts are visible-token estimates calculated with the local `o200k_base` tokenizer. Hidden system instructions and internal tool overhead are excluded. Exact model usage telemetry was unavailable.

## Code quality

All eighteen configurations produced correct iterative implementations:

- Correctly calculate zero-indexed Fibonacci numbers.
- Handle `n = 0` and `n = 1` correctly.
- Raise `ValueError` for negative inputs.
- Include the requested type hints and a docstring.
- Use O(n) time and O(1) additional space.

Terra, GPT-5.5, and GPT-5.4 generally produced the clearest documentation. GPT-5.4 Mini also produced correct code, but its answers contained unnecessary follow-up suggestions that were not requested. Increasing effort did not materially change correctness or the algorithm for this simple task.

## Preferred model

I prefer **Terra** overall because it provided a strong balance of correctness, concise output, and detailed documentation. Among the newly tested models, **GPT-5.5** was the strongest for documentation quality, while GPT-5.4 was more concise. GPT-5.4 Mini was correct but more verbose than necessary.

## Conclusion

All tested models solved the task correctly at every tested effort level. The main differences were documentation detail, extra prose, and measured orchestration time. Higher effort did not provide a meaningful improvement in correctness or algorithm choice for this simple problem.
