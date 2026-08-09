# AI Course Model Comparison

This repository contains the Option B submission for the AI course model-selection and prompt-engineering exercises.

## Model selection guide

The table combines the measured Part 1 benchmark with qualitative ratings from the URL-validation, role-based authentication, and constrained-output exercises.

| Model | Speed* | Code quality | Follows instructions | Security awareness | Consistency | Token efficiency | Best for |
|---|---:|---:|---:|---:|---:|---:|---|
| GPT-5.6 Terra | 22.04 tokens/s | 4/5 | 4/5 | 4/5 | 3/5 | 5/5 | Balanced everyday coding |
| GPT-5.6 Luna | 19.50 tokens/s | 4/5 | 4/5 | 4/5 | 4/5 | 5/5 | High-volume routine code |
| GPT-5.6 Sol | 20.28 tokens/s | 5/5 | 5/5 | 5/5 | 5/5 | 5/5 | Security-critical and complex work |
| GPT-5.5 | 11.35 tokens/s | 4/5 | 4/5 | 4/5 | 4/5 | 3/5 | Deep documentation and analysis |
| GPT-5.4 | 15.74 tokens/s | 4/5 | 4/5 | 4/5 | 4/5 | 4/5 | General engineering with good structure |
| GPT-5.4 Mini | 16.77 tokens/s | 4/5 | 4/5 | 4/5 | 4/5 | 3/5 | Low-cost prototyping and simple CRUD |

\* Speed is weighted visible-token throughput from the Part 1 low/medium/high runs: total visible tokens divided by end-to-end response time. It includes orchestration and concurrency effects, so it is directional rather than pure generation speed. Token counts are local visible-token estimates; hidden system and tool tokens are excluded. See [Part 1](homework_part1.md) for the raw measurements.

The official OpenAI model comparison page is available [here](https://developers.openai.com/api/docs/models/compare); the ratings above are this project's task-specific observations, not universal model scores.

## Recommendations

### GPT-5.6 Terra

Use Terra for everyday application code, structured documentation, and tasks where a strong quality/cost balance matters. It was the fastest measured model in this benchmark and highly token-efficient, but its URL results varied by effort and one medium-effort implementation had a missing import.

### GPT-5.6 Luna

Use Luna for high-volume routine coding, validators, CRUD scaffolding, and other tasks where concise output is valuable. It followed detailed prompts well and produced stable results, but it sometimes applied stricter domain rules than the test specification required, such as rejecting `localhost`.

### GPT-5.6 Sol

Use Sol for security-sensitive code, ambiguous requirements, and tasks where correctness matters more than minimal cost. It was the only model that passed every required URL test at every tested effort level and consistently implemented the security and format constraints; the main limitation is that it may produce more infrastructure detail than a simple task needs.

### GPT-5.5

Use GPT-5.5 for complex explanations, thorough documentation, and professional code reviews. It produced clear, well-documented implementations and strong constrained outputs, but it had the lowest measured visible-token throughput and used more tokens than the GPT-5.6 models in the simple benchmark.

### GPT-5.4

Use GPT-5.4 for general engineering tasks that benefit from a complete, readable implementation with moderate token use. It handled structured prompts well and generated useful security-aware code, but its URL validator made stricter assumptions about `localhost` and some answers used broad exception handling.

### GPT-5.4 Mini

Use GPT-5.4 Mini for inexpensive prototyping, simple CRUD endpoints, and high-volume code completion where the task is well specified. It completed the structured endpoint consistently and was correct on simple benchmarks, but it often added unnecessary follow-up prose and should receive extra review for subtle security and validation assumptions.

## Submission structure

### Exercises

- [Part 1 — Model selection and setup](exercises/part1_model_selection_setup.md)
- [Exercise 1 — Zero-shot URL validation](exercises/exercise1_zero_shot_url_validation.md)
- [Exercise 2 — Specified URL validation](exercises/exercise2_specified_url_validation.md)
- [Exercise 4 — Role-based authentication](exercises/exercise4_role_based_authentication.md)
- [Exercise 5 — Constrained output format](exercises/exercise5_constrained_output.md)

### Results and analysis

- [Cross-exercise analysis](analysis.md)
- [Results index](results/README.md)
- [Part 1 metrics](results/part1_metrics.md)
- [Part 2 URL-validation results](results/part2_url_validation.md)
- [Part 3 authentication results](results/part3_role_auth.md)
- [Part 5 constrained-output results](results/part5_constrained_output.md)
- [Normalized model-output summary](results/model_outputs.md)

## Existing detailed reports

- [Homework Part 1](homework_part1.md)
- [Homework Part 2](homework_part2.md)
- [Homework Part 3](homework_part3.md)
- [Homework Part 5](homework_part5.md)
