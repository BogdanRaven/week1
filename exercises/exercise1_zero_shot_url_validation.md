# Exercise 1 — Zero-Shot URL Validation

## Prompt

> Write a Python function to validate URLs

## Test cases

| URL | Expected |
|---|---|
| `https://example.com` | Valid |
| `http://localhost:8080/path` | Valid |
| `ftp://files.example.com` | Invalid |
| `not-a-url` | Invalid |
| `https://example.com:8080/path?query=value` | Valid |

## Result

The zero-shot prompt produced inconsistent assumptions. Models differed mainly on whether `localhost` should be accepted and how strictly to validate ports and domain labels. Sol was the only model with 5/5 at low, medium, and high effort.

See [the detailed Part 2 report](../homework_part2.md#exercise-1-zero-shot-code-generation) and [the normalized results](../results/part2_url_validation.md).
