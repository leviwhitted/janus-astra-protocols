# Two AI operating protocols

Prepared for Daniel on 2026-09-20.

This packet contains two related protocols:

1. `JANUS_PROTOCOL_EXTERNAL_EDITION.md` explains how to use a second model as a bounded peer while keeping one accountable orchestrator.
2. `ASTRA_OPENAI_OPTIMIZATION_PROTOCOL.md` explains how to route work across current OpenAI models and calibrate those choices against accepted results.
3. `SOURCES.md` records the official OpenAI pages used for the current model claims.

Read Janus first. It defines the review and accountability layer. The Astra/OpenAI protocol then applies a similar discipline to model selection, effort, handoffs, and evaluation.

## Status

- The Janus concept is mature enough for external review and adaptation. Its full local test suite and policy evaluations pass as of 2026-09-20, but green checks do not establish test quality. Assume defects remain. The implementation provides discipline and accounting. It does not create a security boundary.
- The Astra/OpenAI protocol is ready to share as an operating framework. Its model routes are starting hypotheses that need calibration on the recipient's own work.
- Current model names and product guidance were rechecked against official OpenAI documentation on 2026-09-20. Availability may differ across the API, ChatGPT, and Codex.
