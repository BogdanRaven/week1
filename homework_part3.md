# Homework Part 3: Advanced Techniques

## Exercise 4: Role-Based Generation

### Task

Generate a Python authentication function and compare three role assignments:

- **Role A — Generic:** `Write a Python function for user authentication that checks username and password against a database.`
- **Role B — Senior developer:** `You are a senior Python developer with 10 years of experience.` followed by `Write a secure authentication function that checks username and password. Follow security best practices.`
- **Role C — Security expert:** `You are an OWASP security expert specializing in authentication vulnerabilities.` followed by requirements to prevent SQL injection, use password hashing, implement rate limiting, handle timing attacks, follow OWASP authentication guidelines, and explain the security measures with comments.

### Experimental setup

The three prompts were run independently with the same six models used in the earlier exercises:

- GPT-5.6 Terra
- GPT-5.6 Luna
- GPT-5.6 Sol
- GPT-5.5
- GPT-5.4
- GPT-5.4 Mini

All runs used medium reasoning effort. The role prompt was the independent variable; model and effort were held constant for each comparison. The generated code was reviewed statically for the four requested security features. No real credentials, database, or Redis service were used.

Feature labels:

- **Yes** — the generated code visibly implements the feature.
- **Partial** — the feature is delegated to an abstraction or only partly addressed.
- **No** — the feature is absent from the generated implementation.

For prepared SQL, “Partial” means the function calls a repository or callback that is instructed to use parameterized SQL, but the query itself is not shown. For timing protection, “Partial” means the password comparison is designed to be constant-time but the missing-user path can still return earlier.

### Security feature comparison

| Model | Role | Password hashing | Prepared statements | Rate limiting | Timing-attack handling | Notes |
|---|---|---:|---:|---:|---:|---|
| Terra | Generic | Yes | Yes | No | No | bcrypt and SQLAlchemy parameter binding; no throttling or dummy-hash path |
| Terra | Senior developer | Yes | Partial | No | Yes | Argon2 and dummy hash; database safety is delegated to `find_user` |
| Terra | Security expert | Yes | Yes | Yes | Yes | Argon2id, parameterized PostgreSQL query, account/IP limiter, dummy hash |
| Luna | Generic | Yes | Yes | No | Partial | Salted PBKDF2 and `hmac.compare_digest`; nonexistent users return before verification |
| Luna | Senior developer | Yes | Partial | No | Yes | Argon2 and dummy hash; rate limiting is only recommended in surrounding code |
| Luna | Security expert | Yes | Yes | Yes | Yes | Argon2id, SQLite parameters, Redis limiter, dummy verification |
| Sol | Generic | Yes | Yes | No | No | Argon2 and parameterized query; no rate limiter or unknown-user timing defense |
| Sol | Senior developer | Yes | Partial | No | Yes | Argon2 and dummy hash through a repository abstraction; rate limiting is only advised |
| Sol | Security expert | Yes | Yes | Yes | Yes | Argon2id, parameterized PostgreSQL query, Redis limiter, dummy hash |
| GPT-5.5 | Generic | Yes | Yes | No | No | Werkzeug password verification and parameterized SQLite query |
| GPT-5.5 | Senior developer | Yes | Partial | No | Yes | Argon2 and dummy hash; repository/database details are left outside the function |
| GPT-5.5 | Security expert | Yes | Yes | Yes | Yes | Argon2, parameterized SQL, Redis-backed account/IP limits, generic failures |
| GPT-5.4 | Generic | Yes | Yes | No | No | bcrypt and parameterized SQLite query |
| GPT-5.4 | Senior developer | Yes | Partial | No | No | Argon2, but the caller supplies a previously fetched user and no dummy path is shown |
| GPT-5.4 | Security expert | Yes | Yes | Yes | Yes | Argon2, parameterized SQLite query, in-memory limiter, dummy hash |
| GPT-5.4 Mini | Generic | Yes | Yes | No | No | bcrypt and parameterized database query |
| GPT-5.4 Mini | Senior developer | Yes | Yes | No | Yes | Argon2, dummy hash, and a parameterized `user_lookup` example; rate limiting is advice only |
| GPT-5.4 Mini | Security expert | Yes | Yes | Yes | Yes | Argon2id, parameterized SQLite query, in-memory limiter, dummy hash and response padding |

### Aggregate results by role

| Role | Hashing | Prepared SQL shown | Rate limiting implemented | Timing mitigation implemented |
|---|---:|---:|---:|---:|
| Generic | 6/6 | 6/6 | 0/6 | 0/6 |
| Senior developer | 6/6 | 1/6 direct, 5/6 partial | 0/6 | 5/6 |
| Security expert | 6/6 | 6/6 | 6/6 | 6/6 |

### Findings

#### Impact of role specificity

Role specificity had a clear impact on the security content:

1. **Generic role:** The outputs were short and easy to understand. They usually used a password-hashing library and a parameterized query, but they did not implement brute-force protection or a dummy-hash path for unknown users.
2. **Senior-developer role:** The outputs were more polished and commonly added Argon2, input validation, generic errors, hash upgrades, and dummy hashes. However, rate limiting was usually described as something the login endpoint should add later rather than implemented in the function. Database access was also frequently hidden behind a repository interface.
3. **Security-expert role:** The outputs directly addressed all four requested controls. They commonly included Argon2id, parameterized SQL, account/IP rate limits, generic failures, dummy-hash verification, and comments explaining the protections.

The role assignment therefore changed both the breadth of the answer and the number of security controls implemented in the returned code. The detailed requirements in Role C were especially important; the title alone was not the only factor.

#### Code-quality differences

- Generic outputs were the smallest and easiest to adapt, but they were incomplete for security-critical authentication.
- Senior-developer outputs had better abstractions and documentation, but sometimes moved important controls outside the function without implementing them.
- Security-expert outputs were the most complete and best documented. Their tradeoff was complexity: several assumed Redis, PostgreSQL, Argon2, or framework-specific interfaces, and those dependencies would need integration testing.

The security-expert outputs were not automatically production-ready. Some used process-local in-memory rate limiting, while others used a shared Redis limiter. A real deployment would need to verify the database driver, distributed rate-limit behavior, session creation, transport security, MFA, logging, recovery flows, and dependency configuration.

### Best role assignment for security-critical code

**Role C — Security expert** produced the strongest results. It was the only role where every model visibly implemented password hashing, prepared statements, rate limiting, and timing-attack defenses. For production work, the best prompt would combine the security-expert role with explicit interfaces, deployment constraints, dependency versions, tests, and a requirement to identify assumptions and limitations.

### Conclusion

Role-based prompting improved the security focus of generated code, but specificity mattered more than the label alone. A generic request produced a basic authentication skeleton; a senior-developer role improved general quality; and the security-expert role consistently produced the required defense-in-depth controls. Generated authentication code still requires static review, dependency review, integration tests, and security testing before use.
