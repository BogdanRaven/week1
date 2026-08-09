# Exercise 5 — Constrained Output Format

## Full prompt

The full prompt required a Flask `POST /api/users` endpoint, a JSON user-creation contract, typed parameters and returns, a Flask-RESTful `Resource`, username/email/password validation, descriptive 400 errors, bcrypt, SQLAlchemy ORM, endpoint documentation, request/response documentation, a curl example, and a four-part output order.

## Reproducibility test

The exact prompt was run twice independently at medium effort on all six models. Both runs were checked against every constraint.

## Ablation

The code-style, security, and documentation sections were removed while retaining only the endpoint and basic request/response contract. This reduced prompt was run once per model.

## Result

All 12 full generations met the functional requirements. The ablation preserved the route and basic status/body shapes but removed all six models' Flask-RESTful, bcrypt, SQLAlchemy, typed-function, and detailed-validation behavior; five outputs stored passwords in plaintext in demo storage.

See [the detailed Part 5 report](../homework_part5.md) and [the normalized results](../results/part5_constrained_output.md).
