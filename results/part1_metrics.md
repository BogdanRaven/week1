# Part 1 Metrics

Speed is shown as weighted visible-token throughput across the low, medium, and high runs: total visible tokens divided by total end-to-end response time.

| Model | Low total/time | Medium total/time | High total/time | Weighted speed | Avg total tokens |
|---|---:|---:|---:|---:|---:|
| Terra | 104 / 6.084 s | 147 / 5.989 s | 147 / 5.983 s | 22.04 tokens/s | 133 |
| Luna | 121 / 5.442 s | 121 / 6.587 s | 121 / 6.591 s | 19.50 tokens/s | 121 |
| Sol | 121 / 4.806 s | 121 / 6.594 s | 123 / 6.597 s | 20.28 tokens/s | 122 |
| GPT-5.5 | 162 / 9.057 s | 181 / 19.126 s | 194 / 19.132 s | 11.35 tokens/s | 179 |
| GPT-5.4 | 141 / 4.891 s | 141 / 7.106 s | 208 / 19.137 s | 15.74 tokens/s | 163 |
| GPT-5.4 Mini | 246 / 19.141 s | 232 / 7.098 s | 283 / 19.150 s | 16.77 tokens/s | 254 |

All 18 Fibonacci outputs were correct. The times include sub-agent startup, scheduling, and concurrency effects; they are not pure model-generation latency.
