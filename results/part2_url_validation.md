# Part 2 URL Validation Results

## Exercise 1: Zero-shot, correct cases out of 5

| Model | Low | Medium | High |
|---|---:|---:|---:|
| Terra | 5/5 | 4/5 | 4/5 |
| Luna | 5/5 | 5/5 | 4/5 |
| Sol | 5/5 | 5/5 | 5/5 |
| GPT-5.5 | 4/5 | 5/5 | 5/5 |
| GPT-5.4 | 5/5 | 5/5 | 5/5 |
| GPT-5.4 Mini | 5/5 | 5/5 | 4/5 |

## Exercise 2: Specified prompt, correct cases out of 5

| Model | Low | Medium | High |
|---|---:|---:|---:|
| Terra | 4/5 | 2/5 | 5/5 |
| Luna | 4/5 | 4/5 | 5/5 |
| Sol | 5/5 | 5/5 | 5/5 |
| GPT-5.5 | 5/5 | 4/5 | 4/5 |
| GPT-5.4 | 5/5 | 4/5 | 4/5 |
| GPT-5.4 Mini | 5/5 | 4/5 | 5/5 |

The main recurring failure was rejecting `http://localhost:8080/path` because the generated validator required a dotted hostname. Terra's medium-effort specified answer additionally failed at runtime because `ipaddress` was not imported. Low/high specified outputs passed five additional invalid-edge cases covering empty input, `None`, malformed URLs, invalid ports, and invalid domains.
