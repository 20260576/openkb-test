---
sources: [summaries/mezzi-2025-llms-unreliable-for-cti.md, summaries/alevizos-2024-ai-enhanced-cti-processing-pipeline.md, summaries/krawczyk-2026-cti-for-ai-systems.md]
brief: LLM 기반 CTI 자동화는 실제 보고서 길이, 일관성, 과신, 편향 문제 때문에 엄격한 검증이 필요하다.
---

# LLM and AI Reliability in CTI

[[summaries/mezzi-2025-llms-unreliable-for-cti]] is the core source for reliability concerns. It finds that LLM performance degrades on real-size CTI reports and that outputs can be inconsistent and overconfident. This is especially relevant when models extract APT names, CVEs, countries, attack vectors, or other structured facts from long narrative reports.

[[summaries/alevizos-2024-ai-enhanced-cti-processing-pipeline]] acknowledges related governance concerns: AI-driven CTI decisions require transparency, interpretability, bias management, and human validation. [[summaries/krawczyk-2026-cti-for-ai-systems]] expands the reliability problem to AI systems themselves, which introduce new attack surfaces and require CTI frameworks for model and data supply chains.

## Related Documents

- [[summaries/mezzi-2025-llms-unreliable-for-cti]]
- [[summaries/alevizos-2024-ai-enhanced-cti-processing-pipeline]]
- [[summaries/krawczyk-2026-cti-for-ai-systems]]
