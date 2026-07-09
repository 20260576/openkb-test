---
sources: [raw/krawczyk-2026-cti-for-ai-systems.pdf]
brief: AI 시스템을 대상으로 한 위협을 다루기 위해 CTI가 어떻게 확장되어야 하는지 정리한 연구.
doc_type: short
full_text: sources/krawczyk-2026-cti-for-ai-systems.md
---

# Cyber Threat Intelligence for Artificial Intelligence Systems

Krawczyk et al. examine how CTI must evolve when the protected asset is an AI system. Traditional CTI remains useful for organizing threat actors, TTPs, vulnerabilities, and indicators, but AI systems introduce assets and attack surfaces that conventional CTI was not designed to capture. These include training data, models, prompts, embeddings, inference pipelines, deployment environments, and AI supply-chain artifacts.

The paper analyzes the gap between conventional threat intelligence workflows and AI-focused defense. It reviews AI security knowledge sources such as MITRE ATLAS, the AI Incident Database, AVID, OWASP AI resources, Google SAIF, ENISA guidance, and vulnerability/taxonomy work. It argues that an AI-oriented CTI knowledge base should include AI-specific IoCs across supply-chain phases and artifacts, and should support security tools by making those indicators searchable, comparable, and operational.

A key theme is similarity measurement: defenders need ways to compare newly observed AI artifacts with collected indicators. This points beyond simple IoC matching toward semantic, structural, and artifact-aware similarity approaches. The paper identifies gaps and quality issues in current resources and frames future work around practical CTI frameworks tailored to AI systems.

## Related Concepts

- [[concepts/cti-for-ai-systems]]
- [[concepts/ai-supply-chain-indicators]]
- [[concepts/cti-knowledge-representation]]
