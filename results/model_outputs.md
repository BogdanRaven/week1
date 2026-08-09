# Normalized Model-Output Summary

These are the output patterns used in the cross-exercise ratings.

| Model | Part 1 | Part 2 | Part 3 | Part 5 |
|---|---|---|---|---|
| Terra | Correct Fibonacci at all efforts; concise | Strong low/high URL results but a missing import at medium in the specified prompt | Security role implemented all four controls | Full prompt compliant twice; ablation fell back to in-memory storage |
| Luna | Correct Fibonacci at all efforts | Correct supplied cases often, but stricter `localhost` policy in some runs | Security role implemented all four controls | Full prompt compliant twice; concise ORM/bcrypt outputs |
| Sol | Correct Fibonacci at all efforts | 5/5 at every URL effort level | Best security-role consistency; all four controls | Full prompt compliant twice; complete and detailed scaffolding |
| GPT-5.5 | Correct, detailed documentation, more tokens | Strong at low/medium but some `localhost` rejection | Security role implemented all four controls | Full prompt compliant twice; polished, complete examples |
| GPT-5.4 | Correct and relatively concise | Good low-effort results; stricter URL assumptions at higher effort | Security role implemented all four controls | Full prompt compliant twice; detailed but sometimes broad setup |
| GPT-5.4 Mini | Correct but often added extra prose | Strong low/high results; minimal validation in some zero-shot answers | Security role implemented all four controls | Full prompt compliant twice; occasional placeholder/unused imports |
