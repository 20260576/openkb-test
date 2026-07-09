---
sources: [summaries/jin-2024-sharing-cyber-threat-intelligence.md, summaries/mavroeidis-2021-cyber-threat-intelligence-model.md, summaries/mezzi-2025-llms-unreliable-for-cti.md]
brief: STIX 공유는 증가했지만 고수준 CTI, 보안 규칙, 정확성, 완전성 측면에서 품질 문제가 남아 있다.
---

# STIX Sharing and Quality

[[summaries/jin-2024-sharing-cyber-threat-intelligence]] provides the strongest empirical view of STIX sharing quality. It finds that public STIX sharing has grown, but the shared objects are dominated by simple IoCs such as malware signatures and URLs. High-level CTI such as TTPs, threat actors, campaigns, and actionable detection rules is sparse. The paper reports that only 0.09% of indicator data included security rules, and that 19% of threat actor data contained incorrect information.

[[summaries/mavroeidis-2021-cyber-threat-intelligence-model]] explains why this matters structurally. STIX is expressive, but CTI knowledge representation also requires clear semantics, ontology support, interoperability, and reasoning. Without these, structured sharing may still produce ambiguous or weakly queryable knowledge.

[[summaries/mezzi-2025-llms-unreliable-for-cti]] adds that using LLMs to transform narrative CTI into STIX-like structured facts is risky when reports are long and ambiguous. Poor extraction can introduce false positives, false negatives, and overconfident structured claims.

## Related Documents

- [[summaries/jin-2024-sharing-cyber-threat-intelligence]]
- [[summaries/mavroeidis-2021-cyber-threat-intelligence-model]]
- [[summaries/mezzi-2025-llms-unreliable-for-cti]]
