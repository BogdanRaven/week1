# AI Course Homework — Part 2: Progressive Prompt Engineering

Each model was tested at low, medium, and high effort. The same effort level was used across models within each run so that prompt quality and effort could be compared separately.

Models tested:

- Terra
- Luna
- Sol
- GPT-5.5
- GPT-5.4
- GPT-5.4 Mini

The generated functions were executed locally against the required URLs. When a response contained multiple code blocks, the final code block defining the requested function was tested.

## Exercise 1: Zero-Shot Code Generation

### Prompt

> Write a Python function to validate URLs

### Test expectations

| URL | Expected result |
|---|---|
| `https://example.com` | Valid |
| `http://localhost:8080/path` | Valid |
| `ftp://files.example.com` | Invalid |
| `not-a-url` | Invalid |
| `https://example.com:8080/path?query=value` | Valid |

### Medium-effort results

| Model | Function name | Correct cases | Main observation |
|---|---|---:|---|
| Terra | `is_valid_url` | 4/5 | Rejected `localhost` because it required a dot in the hostname. |
| Luna | `is_valid_url` | 5/5 | Correct supplied cases, but did not validate domain labels or port values. |
| Sol | `is_valid_url` | 5/5 | Strong zero-shot validator; checked schemes, hostname, ports, and IDN encoding. |
| GPT-5.5 | `is_valid_url` | 5/5 | Simple and readable, but only checked scheme and network location. |
| GPT-5.4 | `is_valid_url` | 5/5 | Correct supplied cases, but used broad `except Exception` and weak validation. |
| GPT-5.4 Mini | `is_valid_url` | 5/5 | Correct supplied cases, with minimal validation and extra follow-up text. |

### Low/medium/high comparison

| Model | Low | Medium | High |
|---|---:|---:|---:|
| Terra | 5/5 | 4/5 | 4/5 |
| Luna | 5/5 | 5/5 | 4/5 |
| Sol | 5/5 | 5/5 | 5/5 |
| GPT-5.5 | 4/5 | 5/5 | 5/5 |
| GPT-5.4 | 5/5 | 5/5 | 5/5 |
| GPT-5.4 Mini | 5/5 | 5/5 | 4/5 |

The zero-shot prompt left important decisions unspecified: accepted protocols, function name, return type, error behavior, domain rules, port validation, and handling of `None` or empty strings. Higher effort did not consistently improve the result. Some high-effort answers became more restrictive and rejected `localhost`.

## Exercise 2: Improved with Specifications

### Prompt

> Write a Python function to validate URLs with the following requirements:
>
> Function name: `validate_url`
> Parameters: `url (str)`
> Returns: `tuple (bool, str) - (is_valid, error_message)`
>
> Requirements:
> - Accept http and https protocols only
> - Validate domain structure
> - Support optional ports (e.g., `:8080`)
> - Support paths and query parameters
> - Return descriptive error messages for invalid URLs
>
> Include:
> - Type hints
> - Docstring with examples
> - Handle edge cases (empty string, None, malformed URLs)

### Medium-effort results

| Model | Correct cases | Main observation |
|---|---:|---|
| Terra | 2/5 | Valid-domain calls crashed with `NameError` because `ipaddress` was used without being imported. |
| Luna | 4/5 | Correct except it rejected `localhost` because it required at least one dot. |
| Sol | 5/5 | Correctly handled all supplied URLs, including `localhost`, port, path, and query. |
| GPT-5.5 | 4/5 | Correct except it rejected `localhost` as lacking a top-level domain. |
| GPT-5.4 | 4/5 | Correct except it rejected `localhost`. |
| GPT-5.4 Mini | 4/5 | Correct except it rejected `localhost`. |

### Low/medium/high comparison

| Model | Low | Medium | High |
|---|---:|---:|---:|
| Terra | 4/5 | 2/5 | 5/5 |
| Luna | 4/5 | 4/5 | 5/5 |
| Sol | 5/5 | 5/5 | 5/5 |
| GPT-5.5 | 5/5 | 4/5 | 4/5 |
| GPT-5.4 | 5/5 | 4/5 | 4/5 |
| GPT-5.4 Mini | 5/5 | 4/5 | 5/5 |

### Additional edge cases

The low- and high-effort implementations were also tested with empty strings, `None`, `https://`, an invalid port, and an invalid domain. Every new low/high configuration returned an appropriate invalid result and descriptive error for all five additional cases.

The medium-effort outputs also demonstrated the same kinds of edge-case handling, although Terra's missing import caused failures for valid-domain inputs.

### Improvement from specifications

The enhanced prompt substantially improved the output structure:

- Every model used the required function name, `validate_url`.
- Every model returned a `(bool, str)` pair.
- Docstrings included examples and described edge cases.
- Error messages became descriptive instead of returning only `True` or `False`.
- Port parsing and domain validation appeared in every implementation.

Sol was the most consistent model: it passed 5/5 at every effort level. Terra improved from 2/5 at medium to 5/5 at high after including the missing import. GPT-5.5 and GPT-5.4 were strong at low effort but became more restrictive at higher effort by rejecting `localhost`. GPT-5.4 Mini also passed 5/5 at low and high effort.

### Remaining missing features

Even the improved prompt does not fully define several policies:

- Whether `localhost`, private IPs, IPv4, and IPv6 should be accepted
- Whether leading/trailing whitespace should be trimmed or rejected
- Whether URL credentials are allowed
- Exact internationalized-domain and public-suffix rules
- Whether fragments and percent-encoded values require validation
- Whether validation means syntax only or also checking network reachability

## Overall conclusion

Progressive prompt engineering clearly improved the consistency and usefulness of the generated code. The zero-shot prompt produced minimal validators with different implicit assumptions. The specification-rich prompt produced structured return values, descriptive errors, documentation, and broader edge-case handling.

The best overall result was Sol's improved implementation because it passed all five required URL tests at low, medium, and high effort. The main lesson is that detailed requirements improve output quality, but generated code must still be executed and reviewed: Terra's medium-effort answer looked thorough but contained a missing import that caused runtime failures. Higher effort alone did not guarantee better results.
