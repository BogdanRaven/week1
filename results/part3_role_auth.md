# Part 3 Role-Based Authentication Results

Legend: **Yes** means visibly implemented, **Partial** means delegated or incomplete, and **No** means absent.

| Model | Role | Hashing | Prepared SQL | Rate limit | Timing defense |
|---|---|---:|---:|---:|---:|
| Terra | Generic | Yes | Yes | No | No |
| Terra | Senior | Yes | Partial | No | Yes |
| Terra | Security | Yes | Yes | Yes | Yes |
| Luna | Generic | Yes | Yes | No | Partial |
| Luna | Senior | Yes | Partial | No | Yes |
| Luna | Security | Yes | Yes | Yes | Yes |
| Sol | Generic | Yes | Yes | No | No |
| Sol | Senior | Yes | Partial | No | Yes |
| Sol | Security | Yes | Yes | Yes | Yes |
| GPT-5.5 | Generic | Yes | Yes | No | No |
| GPT-5.5 | Senior | Yes | Partial | No | Yes |
| GPT-5.5 | Security | Yes | Yes | Yes | Yes |
| GPT-5.4 | Generic | Yes | Yes | No | No |
| GPT-5.4 | Senior | Yes | Partial | No | No |
| GPT-5.4 | Security | Yes | Yes | Yes | Yes |
| GPT-5.4 Mini | Generic | Yes | Yes | No | No |
| GPT-5.4 Mini | Senior | Yes | Yes | No | Yes |
| GPT-5.4 Mini | Security | Yes | Yes | Yes | Yes |

Role C was the only role with all four controls implemented in every model: 6/6 hashing, 6/6 prepared SQL, 6/6 rate limiting, and 6/6 timing defense.
