# AI Course Homework — Part 2: Progressive Prompt Engineering

All models were tested at **medium effort** so that the comparison focused on prompt quality rather than reasoning-effort settings.

Models tested:

- Terra
- Luna
- Sol
- GPT-5.5
- GPT-5.4
- GPT-5.4 Mini

The generated functions were executed locally against the required URLs.

## Exercise 1: Zero-Shot Code Generation

### Prompt

> Write a Python function to validate URLs

### Test expectations

For this exercise, the five supplied URLs were treated as follows:

| URL | Expected result |
|---|---|
| `https://example.com` | Valid |
| `http://localhost:8080/path` | Valid |
| `ftp://files.example.com` | Invalid |
| `not-a-url` | Invalid |
| `https://example.com:8080/path?query=value` | Valid |

### Results

| Model | Function name | Correct cases | Main observation |
|---|---|---:|---|
| Terra | `is_valid_url` | 4/5 | Rejected `localhost` because it required a dot in the hostname. Also did not validate port values. |
| Luna | `is_valid_url` | 5/5 | Correct test results, but did not validate domain labels or port values. |
| Sol | `is_valid_url` | 5/5 | Strongest zero-shot validator; checked schemes, hostname, ports, and IDN encoding. |
| GPT-5.5 | `is_valid_url` | 5/5 | Simple and readable, but only checked scheme and non-empty network location. |
| GPT-5.4 | `is_valid_url` | 5/5 | Correct supplied cases, but used broad `except Exception` and weak validation. |
| GPT-5.4 Mini | `is_valid_url` | 5/5 | Correct supplied cases, with similarly minimal validation and extra follow-up text. |

### Code quality and missing edge cases

The zero-shot prompt left important decisions unspecified: accepted protocols, function name, return type, error behavior, domain rules, port validation, and handling of `None` or empty strings.

Most models used `urllib.parse`, which is a good starting point but does not by itself validate a URL. The simpler implementations generally did not check:

- Invalid or out-of-range ports
- Domain labels, hyphens, underscores, or consecutive dots
- Whitespace inside URLs
- IP addresses and IPv6 syntax
- `localhost` policy
- Descriptive failure reasons

Sol produced the strongest zero-shot implementation. Terra was more restrictive than the test expectation because it required a dotted hostname.

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

### Results on the required URLs

| Model | Correct cases | Main observation |
|---|---:|---|
| Terra | 2/5 | Valid-domain calls crashed with `NameError` because `ipaddress` was used without being imported. |
| Luna | 4/5 | Correct except it rejected `localhost` because it required at least one dot. |
| Sol | 5/5 | Correctly handled all supplied URLs, including `localhost`, port, path, and query. |
| GPT-5.5 | 4/5 | Correct except it rejected `localhost` as lacking a top-level domain. |
| GPT-5.4 | 4/5 | Correct except it rejected `localhost`; it also stripped and accepted leading whitespace. |
| GPT-5.4 Mini | 4/5 | Correct except it rejected `localhost`; it also stripped and accepted leading whitespace. |

### Additional edge cases

The improved functions were also tested with empty strings, `None`, `https://`, an invalid port, an invalid domain, and leading whitespace.

Most models returned descriptive errors for these cases. Sol handled the broadest set of cases, including `localhost`, IP addresses, IPv6, ports, credentials, whitespace, and internationalized domains. Terra's missing import was the most serious implementation defect. GPT-5.4 and GPT-5.4 Mini silently accepted a leading-space URL because they called `.strip()` before validation.

### Improvement from specifications

The enhanced prompt substantially improved the output structure:

- Every model used the required function name, `validate_url`.
- Every model returned a `(bool, str)` pair.
- Docstrings included examples and described edge cases.
- Error messages became descriptive instead of returning only `True` or `False`.
- Port parsing and domain validation appeared in every implementation.

The specifications improved clarity most noticeably for GPT-5.4 and GPT-5.4 Mini, which produced much more complete validators than in the zero-shot exercise. Sol benefited most in practical correctness because it supported `localhost` and several address formats. Terra produced a detailed design but failed at runtime due to the missing import.

### Remaining missing features

Even the improved prompt does not fully define several policies:

- Whether `localhost`, private IPs, IPv4, and IPv6 should be accepted
- Whether leading/trailing whitespace should be trimmed or rejected
- Whether URL credentials are allowed
- Exact internationalized-domain and public-suffix rules
- Whether fragments and percent-encoded values require validation
- Whether validation means syntax only or also checking network reachability

## Conclusion

Progressive prompt engineering clearly improved the consistency and usefulness of the generated code. The zero-shot prompt produced several minimal validators with different implicit assumptions. The specification-rich prompt produced structured return values, descriptive errors, documentation, and broader edge-case handling.

The best overall result was Sol's improved implementation because it passed all five required URL tests and handled additional host and port cases. The main lesson is that detailed requirements improve output quality, but generated code must still be executed and reviewed: Terra's enhanced answer looked thorough but contained a missing import that caused runtime failures.
