---
sources: [raw/mezzi-2025-llms-unreliable-for-cti.pdf]
brief: 실제 크기의 CTI 보고서에서 LLM의 추출 성능, 일관성, 신뢰도 보정 한계를 평가한 연구.
doc_type: short
full_text: sources/mezzi-2025-llms-unreliable-for-cti.md
---

# Large Language Models Are Unreliable for Cyber Threat Intelligence

Mezzi, Massacci, and Tuma challenge optimistic claims that LLMs can reliably automate CTI extraction. Their core argument is that prior evaluations often use sentences or short paragraphs, while real CTI reports are much longer and contain distracting context. The paper evaluates LLMs on realistic CTI tasks using 350 threat intelligence reports and STIX-structured ground truth.

The evaluation covers zero-shot, few-shot, and fine-tuning settings, and it measures not only task performance but also consistency and confidence calibration. The authors report that LLMs cannot guarantee sufficient performance on full-size CTI reports. Few-shot learning and fine-tuning only partially improve results, and in some cases performance remains weak for entities such as CVEs and attack vectors.

The paper also emphasizes operational risk. CTI workflows require repeatability, because inconsistent extraction can affect patch management, prioritization, and incident response. The authors find that LLM outputs can be inconsistent and overconfident, which is especially dangerous when analysts treat generated CTI as structured evidence. The conclusion is not that LLMs have no role in CTI, but that they need careful evaluation on real reports, calibration checks, and human oversight before operational use.

## Related Concepts

- [[concepts/llm-and-ai-reliability-in-cti]]
- [[concepts/human-ai-collaboration-in-cti]]
- [[concepts/cti-automation-and-standards]]
