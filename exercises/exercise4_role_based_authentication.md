# Exercise 4 — Role-Based Authentication

## Role prompts

### Role A — Generic

> Write a Python function for user authentication that checks username and password against a database.

### Role B — Senior developer

> You are a senior Python developer with 10 years of experience.
>
> Write a secure authentication function that checks username and password. Follow security best practices.

### Role C — Security expert

> You are an OWASP security expert specializing in authentication vulnerabilities.
>
> Write a Python authentication function that prevents SQL injection, uses proper password hashing, implements rate limiting, handles timing attacks, follows OWASP authentication guidelines, and includes comments explaining the security measures.

## Evaluation

Each role was run at medium effort on all six models. Outputs were reviewed for password hashing, prepared statements, rate limiting, and timing-attack handling.

## Result

Role C implemented all four controls in 6/6 models. Role A implemented hashing and prepared SQL but no rate limiting or timing defense; Role B improved hashing and timing behavior but still implemented rate limiting in 0/6 outputs.

See [the detailed Part 3 report](../homework_part3.md) and [the normalized role matrix](../results/part3_role_auth.md).
