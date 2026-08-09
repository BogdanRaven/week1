# Cross-Exercise Analysis

## Executive summary

The experiments show that model choice matters, but prompt quality and evaluation criteria matter just as much. Sol was the strongest overall model for this task set: it was correct on every URL test at low, medium, and high effort, implemented all four requested authentication controls under the security-expert role, and followed the constrained endpoint format consistently.

The main practical lesson is to choose the smallest model that reliably meets the task's quality bar, then spend prompt detail on explicit requirements, edge cases, security controls, and output structure. Higher reasoning effort alone was not monotonic: it sometimes improved a result, but it also made some validators reject `localhost` or become more restrictive.

## Evidence by exercise

### Part 1: Model selection benchmark

All models generated correct Fibonacci implementations at all effort levels. Terra had the highest weighted visible-token throughput at 22.04 tokens/s and an average of about 133 visible tokens per response. GPT-5.5 was the slowest in this orchestration measurement at 11.35 tokens/s and averaged about 179 visible tokens; GPT-5.4 Mini averaged about 254 because of additional prose in the complete responses.

These are not pure serving benchmarks. The response times include sub-agent startup, scheduling, and concurrency effects, and the token counts exclude hidden system/tool overhead. They are useful for this homework's relative comparison but should not be treated as a production latency or pricing estimate.

### Part 2: Progressive prompt engineering

The zero-shot URL prompt left protocol, domain, port, error, and edge-case policies unspecified. The models therefore made different assumptions, especially about whether `localhost` is a valid hostname.

The specification-rich prompt improved structure across all models: the requested function name, tuple return type, docstring, descriptive errors, and edge-case handling appeared consistently. Sol was the most robust, scoring 5/5 at every effort level. Terra's medium-effort answer exposed an important failure mode: code can look detailed while still failing at runtime because of a missing import.

### Part 3: Role-based authentication

Role C, the security-expert prompt, was the only role where every model visibly implemented password hashing, prepared statements, rate limiting, and timing-attack defenses. The generic prompt usually covered hashing and parameterized queries but omitted brute-force and username-enumeration defenses. The senior-developer prompt improved hashing, generic errors, and dummy-hash use, but usually left rate limiting as a recommendation for the surrounding login endpoint.

This indicates that explicit security requirements are more reliable than a broad request to “follow best practices.” It also shows why generated authentication code needs review: some outputs assumed Redis or PostgreSQL, while others used process-local in-memory rate limiting.

### Part 5: Constrained output format

The exact structured prompt produced 12/12 functionally compliant generations across two independent runs for every model. The models consistently included the Flask-RESTful resource, validation rules, bcrypt, SQLAlchemy ORM, documentation, response codes, and curl example.

When the code-style, security, and documentation sections were removed, all six models retained only the basic endpoint contract. None used bcrypt, SQLAlchemy ORM, Flask-RESTful, type hints, or the requested length/email validation. Five outputs stored passwords in plaintext in an in-memory list; the sixth omitted password storage. This ablation is the clearest evidence that constraints materially shaped the safety and completeness of the generated code.

## Model-by-model assessment

### GPT-5.6 Terra

Terra is a strong default for everyday coding. It was fast and token-efficient, and it handled structured endpoint prompts well, but its URL quality varied with effort and one medium-effort implementation failed because of a missing import.

### GPT-5.6 Luna

Luna is useful for concise, repeated coding tasks and high-volume scaffolding. It was generally stable and followed detailed requirements, but it sometimes imposed stricter validation policies than the benchmark intended.

### GPT-5.6 Sol

Sol is the best choice from this study for complex or security-sensitive tasks. It had the strongest URL consistency, full role-C security coverage, and reliable constrained-output compliance; its main tradeoff is that it may provide more implementation detail than a very simple task requires.

### GPT-5.5

GPT-5.5 is well suited to deep explanations, documentation, and complex professional coding tasks. It produced polished answers and strong structured endpoints, but it was slower and more token-heavy in the simple benchmark than the GPT-5.6 family.

### GPT-5.4

GPT-5.4 is a good general engineering model when a complete and readable solution is needed. It followed structured requirements well and showed good security awareness, although URL validation assumptions and broad exception handling still require review.

### GPT-5.4 Mini

GPT-5.4 Mini is appropriate for low-cost prototyping, simple CRUD, and routine code completion. It was correct on simple tasks and complied with the structured endpoint prompt, but it generated extra prose and needs closer review for subtle security and policy decisions.

## Selection recommendations

| Scenario | Recommended model | Reason |
|---|---|---|
| Security-critical authentication or authorization | GPT-5.6 Sol | Best observed security-role coverage and consistency |
| Complex code review or architecture explanation | GPT-5.5 or GPT-5.6 Sol | Strong documentation and reasoning depth |
| Balanced everyday development | GPT-5.6 Terra | Best measured speed/token balance with good quality |
| High-volume routine validators or CRUD | GPT-5.6 Luna or GPT-5.4 Mini | Concise, repeatable, and suitable for well-specified work |
| Structured API scaffolding | GPT-5.6 Sol, Terra, or GPT-5.4 | All followed the detailed contract well; choose by cost/quality needs |

## Limitations

- Speed was measured through end-to-end sub-agent calls, not direct model-generation telemetry.
- Token counts were visible-token estimates using the local tokenizer; hidden system and reasoning tokens were unavailable.
- Security and constrained-output code was reviewed statically rather than run against real Flask, SQLAlchemy, Redis, or PostgreSQL services.
- The model ratings are task-specific and based on a small homework benchmark, not a universal ranking.
- The prompt runs were performed at different effort levels for Part 2 and at medium effort for Parts 3 and 5; effort should be included in any future controlled comparison.
