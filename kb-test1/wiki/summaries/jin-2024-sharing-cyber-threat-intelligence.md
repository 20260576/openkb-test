---
sources: [raw/jin-2024-sharing-cyber-threat-intelligence.pdf]
brief: 대규모 STIX 데이터를 분석해 CTI 공유의 양, 속도, 범위, 품질을 평가한 연구.
doc_type: short
full_text: sources/jin-2024-sharing-cyber-threat-intelligence.md
---

# Sharing Cyber Threat Intelligence: Does It Really Help?

Jin et al. empirically evaluate whether public CTI sharing is sufficient and useful in practice. The paper introduces CTI-Lense, a framework for collecting and analyzing public STIX data. The authors collected about 6 million STIX objects from ten publicly available CTI sources over roughly nine years, from October 31, 2014 to April 10, 2023. They evaluate shared CTI along four dimensions: volume, timeliness, coverage, and quality.

The findings are mixed. STIX sharing has increased over time, but the daily volume is still small compared with the scale of new malware, malicious IPs, domains, and URLs. Coverage is narrow: malware signatures and URLs dominate, while high-value CTI such as TTPs, threat actors, campaigns, and security rules is much less represented. The paper reports that only 0.09% of indicator data provided security rules usable for automated detection.

Timeliness differs by object type. URLs are often shared quickly, with about 72% shared earlier than or on the same day as VirusTotal, but malware signature sharing is slower. Quality issues are also significant: the paper reports incorrect information in 19% of threat actor data, and many STIX objects encode information as narrative descriptions rather than precise structured attributes. The conclusion is not that CTI sharing is useless, but that effective scalable sharing requires better producer discipline, richer high-level intelligence, quality control, deduplication, and more consistent use of STIX fields.

## Related Concepts

- [[concepts/stix-sharing-and-quality]]
- [[concepts/cti-automation-and-standards]]
- [[concepts/cti-knowledge-representation]]
