---
sources: [summaries/krawczyk-2026-cti-for-ai-systems.md, summaries/jin-2024-sharing-cyber-threat-intelligence.md]
brief: AI 공급망 IoC는 모델·데이터·artifact 유사도와 lifecycle 단계별 관찰값을 포함해야 한다.
---

# AI Supply-Chain Indicators

[[summaries/krawczyk-2026-cti-for-ai-systems]] highlights that AI-focused CTI should define indicators across supply-chain phases and artifacts. Unlike traditional IoCs such as IP addresses, file hashes, and URLs, AI indicators may involve model files, training data properties, prompt behavior, embedded malicious logic, suspicious hosted models, or similarity between newly observed artifacts and known malicious examples.

[[summaries/jin-2024-sharing-cyber-threat-intelligence]] shows why this is hard: even conventional STIX sharing struggles with richness, precision, and structured detection rules. AI supply-chain indicators will require more precise schemas and better producer guidance to avoid becoming narrative-only records.

## Related Documents

- [[summaries/krawczyk-2026-cti-for-ai-systems]]
- [[summaries/jin-2024-sharing-cyber-threat-intelligence]]
