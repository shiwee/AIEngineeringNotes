# AI Engineering Notes

Practical, concise notes on engineering in AI adoption — from architecture
patterns and prompt engineering to evaluation, deployment, and production
reliability.

🌐 **Read online:** [shiwee.github.io/AIEngineeringNotes](https://shiwee.github.io/AIEngineeringNotes)

## Topics

| Topic | Description |
|---|---|
| **System Architecture** | RAG, agents, pipelines, and orchestration patterns |
| **Prompt Engineering** | Few-shot, chain-of-thought, structured outputs, versioning |
| **Evaluation** | LLM-as-judge, benchmarks, red-teaming |
| **Deployment** | Serving, latency, streaming, batching, cost management |
| **Safety & Guardrails** | Content moderation, output validation, rate limiting |
| **Observability** | Tracing, metrics, logging for AI systems |

## Engineering Principles

1. **Start with evals.** Define measurable success criteria before writing a single prompt.
2. **Treat prompts as code.** Version-control them and test changes in CI.
3. **Design for graceful degradation.** AI components will fail — build fallback paths.
4. **Observe everything.** Log inputs, outputs, latency, and token usage from day one.
5. **Keep humans in the loop** for high-stakes decisions.

## Contributing

Contributions are welcome! Open an issue or pull request to suggest notes, corrections, or new topics.

## License

[MIT](LICENSE)
