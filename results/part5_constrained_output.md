# Part 5 Constrained Output Results

## Full prompt

The exact prompt was run twice at medium effort on every model. Each model passed the functional endpoint, response, validation, bcrypt, SQLAlchemy, documentation, and output-order checks in both runs.

| Constraint group | Full run 1 | Full run 2 |
|---|---:|---:|
| Endpoint/request/response contract | 6/6 | 6/6 |
| Flask-RESTful Resource | 6/6 | 6/6 |
| Type hints for endpoint inputs/returns | 6/6 | 6/6 |
| Username/email/password validation | 6/6 | 6/6 |
| bcrypt | 6/6 | 6/6 |
| SQLAlchemy ORM | 6/6 | 6/6 |
| Descriptive errors | 6/6 | 6/6 |
| Documentation and curl example | 6/6 | 6/6 |
| Four-part output order | 6/6 | 6/6 |

## Reduced-constraint ablation

| Feature | Result out of 6 |
|---|---:|
| Endpoint and basic response contract | 6/6 |
| Detailed length/email validation | 0/6 |
| Flask-RESTful Resource | 0/6 |
| Type hints | 0/6 |
| bcrypt | 0/6 |
| SQLAlchemy ORM | 0/6 |
| Endpoint/request/response documentation | 0/6 |
| curl example | 6/6 |
| Plaintext password storage in demo | 5/6 |

The full outputs were highly reproducible at the requirement level but varied in app scaffolding, dependency style, email regex, and duplicate-user handling.
