---
sources: [summaries/jin-2024-sharing-cyber-threat-intelligence.md, summaries/mavroeidis-2021-cyber-threat-intelligence-model.md, summaries/mezzi-2025-llms-unreliable-for-cti.md, summaries/alevizos-2024-ai-enhanced-cti-processing-pipeline.md]
brief: CTI 자동화는 STIX/TAXII 같은 표준과 기계가 읽을 수 있는 표현에 의존한다.
---

# CTI Automation and Standards

Across the KB, CTI automation depends on converting natural-language threat information into structured, machine-readable forms. [[summaries/mavroeidis-2021-cyber-threat-intelligence-model]] frames this as a knowledge representation problem: defenders need expressive and unambiguous standards, taxonomies, ontologies, and schemas to support correlation, reasoning, and automated response. [[summaries/jin-2024-sharing-cyber-threat-intelligence]] shows the same issue empirically: even when STIX is available, producers often underuse advanced fields and encode information narratively.

The automation promise appears in two directions. [[summaries/alevizos-2024-ai-enhanced-cti-processing-pipeline]] proposes AI support throughout the CTI processing pipeline, from ingestion to mitigation recommendations and resilience verification. [[summaries/mezzi-2025-llms-unreliable-for-cti]] warns that LLM-based automation can fail on real-size reports and should not be treated as reliable extraction without evaluation, calibration, and human oversight.

## Synthesis

The documents agree that CTI scale requires automation, but they disagree with naive automation. Standards such as STIX/TAXII are necessary but not sufficient; they must be used consistently and richly. AI can help process volume, but its outputs need validation, structured grounding, and governance.

## Related Documents

- [[summaries/jin-2024-sharing-cyber-threat-intelligence]]
- [[summaries/mavroeidis-2021-cyber-threat-intelligence-model]]
- [[summaries/mezzi-2025-llms-unreliable-for-cti]]
- [[summaries/alevizos-2024-ai-enhanced-cti-processing-pipeline]]
