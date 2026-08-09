# Homework Part 5: Advanced Techniques

## Exercise 5: Constrained Output Format

### Task

Generate a Flask REST API endpoint with exact structural, validation, security, and documentation constraints. The endpoint must create a user at `POST /api/users` and return JSON responses for success and errors.

### Experimental setup

The exact structured prompt was run twice independently for each of the six models used in the earlier exercises. A third run used a reduced prompt with the code-style, security, and documentation sections removed.

All runs used medium reasoning effort. The model and effort were held constant; the prompt constraints were the variable. Outputs were reviewed statically for constraint compliance and code quality. No Flask application, database, or external dependencies were installed or executed.

The experiment design follows the official OpenAI Docs recommendation to change one group of prompt instructions and rerun the same representative evaluation: [official OpenAI model guidance](https://developers.openai.com/api/docs/guides/latest-model).

Models:

- GPT-5.6 Terra
- GPT-5.6 Luna
- GPT-5.6 Sol
- GPT-5.5
- GPT-5.4
- GPT-5.4 Mini

### Full structured prompt

```text
Generate a Python Flask REST API endpoint with EXACT adherence to these constraints:

SPECIFICATIONS:
- Endpoint: POST /api/users
- Purpose: Create a new user
- Request body: JSON with {username: str, email: str, password: str}
- Response: 201 Created with {id: int, username: str, email: str}
- Error: 400 Bad Request with {error: str}

CODE STYLE:
- Use type hints for all parameters and returns
- Include Flask-RESTful Resource class
- Validation: username (3-20 chars), email (valid format), password (8+ chars)
- Error handling: return descriptive messages

SECURITY:
- Hash passwords with bcrypt
- Validate all inputs
- SQL injection prevention (use SQLAlchemy ORM)

DOCUMENTATION:
- Add docstring with endpoint description
- Document request/response format
- Include example usage

OUTPUT FORMAT:
1. Imports
2. Resource class definition
3. Registration code
4. Example curl request
```

### Full-prompt compliance

Both full runs from every model satisfied the functional constraints. The counts below are per run, out of six models.

| Constraint | Full run 1 | Full run 2 | Result |
|---|---:|---:|---|
| `POST /api/users` endpoint | 6/6 | 6/6 | Stable |
| Create-user purpose and JSON request fields | 6/6 | 6/6 | Stable |
| 201 response with `id`, `username`, and `email` | 6/6 | 6/6 | Stable |
| 400 response with an `error` field | 6/6 | 6/6 | Stable |
| Type hints for endpoint inputs and returns | 6/6 | 6/6 | See note below |
| Flask-RESTful `Resource` class | 6/6 | 6/6 | Stable |
| Username length 3–20 | 6/6 | 6/6 | Stable |
| Email-format validation | 6/6 | 6/6 | Stable |
| Password length of at least 8 | 6/6 | 6/6 | Stable |
| bcrypt password hashing | 6/6 | 6/6 | Stable |
| SQLAlchemy ORM usage | 6/6 | 6/6 | Stable |
| Descriptive 400 error handling | 6/6 | 6/6 | Stable |
| Endpoint docstring | 6/6 | 6/6 | Stable |
| Request/response documentation | 6/6 | 6/6 | Stable |
| Example curl request | 6/6 | 6/6 | Stable |
| Requested four-part output order | 6/6 | 6/6 | Stable |

The type-hint result treats `self` as the conventional implicit instance parameter. Several outputs did not annotate `self`, so a literal interpretation of “all parameters” would reduce exact compliance. Endpoint parameters and return values were consistently annotated.

### Code-quality review

| Model | Full-run quality observations |
|---|---|
| Terra | Clean validation, bcrypt hashing, ORM persistence, duplicate handling, and documentation. One run used imported application/model objects; the other left app setup outside the snippet. |
| Luna | Clear helper-based or inline validation and good descriptive errors. Both runs used a Flask-Bcrypt extension and imported an existing SQLAlchemy model/database. |
| Sol | Strongest email validation in one run and complete app/model setup in the other. Both handled unique-user conflicts and returned the requested public fields. |
| GPT-5.5 | Complete, readable examples with full model/app setup, ORM queries, bcrypt, and curl documentation. |
| GPT-5.4 | Detailed and runnable-looking examples with model definitions, app setup, validation, and database error handling. One run included `debug=True`, which should not be enabled in production. |
| GPT-5.4 Mini | Followed the constraints well and included strong validation. One run used placeholder imports such as `your_app`, and another included an unused `Session` import. |

Common quality strengths were separation of validation from persistence, bcrypt hashing before storage, ORM-based duplicate checks, rollback on database errors, generic JSON error shapes, and avoiding the password in success responses.

Common integration limitations were placeholder module paths, differing Flask-Bcrypt versus direct `bcrypt` dependencies, simplified email regular expressions, and inconsistent application initialization. These are implementation details to resolve before deployment, not prompt-compliance failures.

### Reproducibility

The two full runs were highly reproducible at the requirement level:

- All 12 full generations preserved every listed functional constraint.
- Every output used the same broad architecture: a Flask-RESTful resource, JSON parsing, field validation, bcrypt hashing, SQLAlchemy persistence, a 201 success tuple, 400 error tuples, and a curl example.
- Exact code was not identical. Models varied in whether they defined the `User` model inline or imported it, whether they used `bcrypt` or `Flask-Bcrypt`, how they validated email, and how they handled duplicate records.
- Terra, Luna, Sol, GPT-5.5, and GPT-5.4 were highly similar across their two full runs. GPT-5.4 Mini was also requirement-stable but varied more in scaffolding and placeholder imports.

Therefore, reproducibility was high for the constrained behavior and moderate for the surrounding project structure. The output format constrained the answer shape, while the detailed requirements constrained the important implementation choices.

### Reduced-constraint ablation

The reduced prompt kept only the endpoint, basic request/response contract, and curl output:

```text
Generate a Python Flask REST API endpoint.

- Endpoint: POST /api/users
- Purpose: Create a new user
- Request body: JSON with {username: str, email: str, password: str}
- Response: 201 Created with {id: int, username: str, email: str}
- Error: 400 Bad Request with {error: str}

Output:
1. Python endpoint code
2. Example curl request
```

| Feature | Ablation result out of 6 | Observation |
|---|---:|---|
| Correct `POST /api/users` route | 6/6 | Preserved by the basic specification |
| 201 success and 400 error shape | 6/6 | Preserved by the basic specification |
| Required fields read from JSON | 6/6 | Preserved, generally with type/non-empty checks |
| Username 3–20 validation | 0/6 | Length constraint disappeared |
| Valid email-format validation | 0/6 | Models only checked that email was present/string-like |
| Password 8+ validation | 0/6 | Models only checked that password was present/string-like |
| Flask-RESTful `Resource` class | 0/6 | All used a plain Flask route/decorator |
| Type hints | 0/6 | Functions were untyped |
| bcrypt | 0/6 | Removed from every output |
| SQLAlchemy ORM | 0/6 | Every model switched to in-memory list storage |
| Endpoint docstring and format docs | 0/6 | Removed from every output |
| curl example | 6/6 | Explicitly retained in the reduced prompt |

Five of the six ablation outputs stored the password in plaintext in the in-memory user list. Terra omitted password storage altogether and only left a comment saying that a real application should hash it. This is an important safety regression caused by removing the explicit security constraints.

### Findings

1. Detailed constraints produced strong consistency. Every full run included the requested framework, validation rules, bcrypt, SQLAlchemy ORM, documentation, and output structure.
2. The output-format requirement made the responses easy to inspect and compare. Each full response clearly separated imports, the resource class, registration, and curl usage.
3. Removing the security and technology constraints caused all models to fall back to an in-memory demo. Most also stored plaintext passwords, despite mentioning that production code should hash them.
4. Constraints improved completeness, but they did not eliminate integration issues. A generated snippet can satisfy the checklist while still requiring dependency, typing, database, and deployment review.
5. Re-running the exact prompt produced stable requirements but not identical source code. This is acceptable reproducibility for code generation, provided the evaluation checks behavior and constraints rather than exact text.

### Conclusion

The structured prompt was substantially more reliable than the reduced prompt. Explicit technologies, validation rules, security requirements, documentation requirements, and output ordering all materially affected the generated implementation. The full prompt achieved 12/12 requirement-complete runs at the functional level; the ablation preserved only the basic endpoint contract and omitted the security-critical details.
