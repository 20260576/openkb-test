


Cyber Threat Intelligence for
Artificial Intelligence Systems
Natalia Krawczyk, Mateusz Szczepkowski, Adrian Brodzik, and Krzysztof Bocianiak
Orange Innovation Poland
Abstract
As artificial intelligence (AI) becomes deeply embedded in critical services and everyday products,
it is increasingly exposed to security threats which traditional cyber defenses were not designed to
handle.
In this paper, we investigate how cyber threat intelligence (CTI) may evolve to address
attacks that target AI systems. We first analyze the assumptions and workflows of conventional threat
intelligence with the needs of AI-focused defense, highlighting AI-specific assets and vulnerabilities.
We then review and organize the current landscape of AI security knowledge. Based on this, we
outline what an AI-oriented threat intelligence knowledge base should contain, describing concrete
indicators of compromise (IoC) for different AI supply-chain phases and artifacts, and showing how
such a knowledge base could support security tools. Finally, we discuss techniques for measuring
similarity between collected indicators and newly observed AI artifacts.
The review reveals gaps
and quality issues in existing resources and identifies potential future research directions toward a
practical threat intelligence framework tailored to AI.
1
Introduction
Cyber attacks such as ransomware, phishing, and
social engineering have increased in recent years,
forcing both companies and public institutions to
pay more attention to security [1]. Getting exact
numbers on global cyber attacks is difficult, as many
go unreported and attack definitions may vary;
but recent research gives us good estimates and
shows clear trends. For instance, one study found
11,497 cybersecurity incidents worldwide from Oc-
tober 2023 to March 2025, covering 106 attack types
across 257 countries [2]. Additionally, forecasts pre-
dict 1,782-2,080 incidents per quarter through 2026,
with nearly half affecting multiple countries.
Another dataset [3] collected 77,623 cyber at-
tack records from 225 countries over 14 months
(Oct 2022-Dec 2023) using AI data collection meth-
ods. It includes various attack types like spam, ran-
somware, exploits, and web threats, and shows how
different threats appear around the world.
Beyond the growing number of attacks, their
impact is getting worse. Estimates suggest cyber
crime could cost around $10.5 trillion per year by
2025, up from roughly $3 trillion in 2015 [4]. The
most affected sectors include finance, energy, and
healthcare, where attacks can interrupt essential
services, expose sensitive data, and generate both
financial losses and reputational damage.
These
trends push organisations to strengthen security
and invest in better protection measures.
As cyber threats grow more complex, organiza-
tions increasingly rely on cyber threat intelligence
(CTI) to better understand, detect, and prepare for
attacks. CTI refers to proactive identification and
analysis of cyber threats. It is the process of col-
lecting, analyzing, and sharing information about
cyber threats to help organizations anticipate, de-
tect, and respond to cyber attacks [5]. CTI includes
information about threat actors, their tactics, tech-
niques, and procedures (TTPs), vulnerabilities, and
indicators of compromise (IOCs) [5].
As artificial intelligence (AI) technology evolves
quickly, the threat landscape is evolving as well.
AI systems create new attack surfaces, from tradi-
tional machine learning (ML) models to large gener-
ative models. This gives attackers new ways to de-
velop attack techniques that do not fit existing CTI
categories.
Recent empirical studies and security
databases highlight the significant scale of attacks
on AI and ML systems. For example, an analysis
using the MITRE ATLAS AI Model Vulnerabili-
ties Dataset recorded 500 adversarial attacks, with
53.6% targeting model-level vulnerabilities, 35.2%
data-level, and 11.2% deployment-level [6].
An-
other study documented 89 real-world ML attack
scenarios from the MITRE ATLAS and AI Incident
Database, showing that convolutional neural net-
works (CNNs) are among the most attacked mod-
els [7]. These numbers represent only a subset of
reported incidents, as many attacks go unreported
or are discovered post-deployment.
1
arXiv:2603.05068v1  [cs.CR]  5 Mar 2026



This creates a need for CTI frameworks de-
signed for AI-related threats.
Such frameworks
would require defining new types of IoCs, and at-
tack patterns that capture risks unique to AI sys-
tems, extending beyond the scope of conventional
IoCs or TTP taxonomies. As a result, developing
CTI approaches that keep up with the dynamics of
AI-driven threats is becoming more important for
maintaining effective defenses.
In this work, we examine how CTI practices
need to change when applied to AI systems. While
traditional CTI provides a solid foundation for col-
lecting and analyzing threat information, it was not
designed for AI-specific risks. We therefore exam-
ine which concepts, models, and workflows from
traditional CTI remain useful, and where new ap-
proaches are required. We analyze existing sources
of knowledge that can support CTI for AI, includ-
ing public datasets, threat repositories, and indus-
try reports. Based on this analysis, we discuss what
types of IoCs should be used in AI-oriented CTI and
how they can help organizations detect, respond to,
and anticipate attacks targeting AI systems.
1.1
Purpose
This study aims to collect and organize state of the
art (SoTA) knowledge to support developing a CTI
framework designed for AI systems. We focus on
understanding how CTI practices need to adapt for
AI, where threats differ significantly from typical
cybersecurity scenarios. Our research questions are:
• RQ1: What are the differences between clas-
sical CTI and CTI for AI?
• RQ2: What sources can be used to build a
CTI for AI knowledge base? How reliable are
these data sources?
• RQ3: How would a CTI for AI knowledge base
benefit tools designed to protect AI?
• RQ4: How to measure the similarity between
collected IoCs and potentially malicious AI
artifacts? How to effectively query the CTI
for AI knowledge base?
1.2
Methodology
This review was conducted following established
guidelines for systematic literature reviews [8]. The
process consisted of the following steps:
1. Search strategy – we identified several rel-
evant keywords:
• Cyber threat intelligence,
• CTI for AI,
• AI incidents,
• AI incidents database,
• AI cyber threats,
• AI vulnerabilities.
We used them to search academic databases
such as Google Scholar and Scopus. In addi-
tion, references cited in the identified papers
were further explored to ensure comprehen-
sive coverage of relevant literature.
2. Analysis and extraction – each paper was
carefully read and analyzed to identify infor-
mation related to CTI in the context of AI,
including proposed frameworks, data sources,
indicators of compromise, and applications in
security tools.
3. Synthesis – insights from the analyzed pa-
pers were structured and organized into a
literature review, highlighting directions for
adapting CTI practices to AI-specific threats.
2
AI Threats and Defenses
Integrating AI into digital systems has changed
both cyber defense and cyber crime.
While AI
helps improve threat detection and response, it also
gives attackers new ways to create harder-to-detect
attacks, changing the nature of digital risk.
To-
day, AI-generated cyber threats are becoming one
of the most pressing challenges. Using techniques
from computer vision, natural language processing,
and machine learning, attackers can create or en-
hance cyber attacks that mislead users, bypass tra-
ditional security, and operate at previously impossi-
ble scales. Cyber criminals now automate malware
production, generate highly targeted phishing mes-
sages that mimic official communication, and dis-
tribute these phishing campaigns at high speed and
large scale [9]. They also misuse deepfake technolo-
gies, such as generative adversarial network-based
(GAN) and diffusion-based audio and video gen-
eration, to deceive victims more convincingly, and
they enhance denial-of-service attacks by automat-
ing and optimizing traffic generation. With easily
accessible AI frameworks and resources, even non-
technical people may conduct complex attacks [9].
At the same time, AI is increasingly used in cy-
bersecurity defense. AI, especially machine learning
and deep learning (DL), can analyze huge amounts
of network data to detect unusual patterns, flagging
potential cyber threats faster and more accurately
than traditional methods [8], [10], [11]. AI systems
can automate responses to certain types of attacks,
reducing response time and limiting damage. They
can also process and connect threat data from mul-
tiple sources, providing useful insights and predict-
ing emerging threats [12].
2



While AI-driven defenses offer better detection
and response, they also introduce new vulnerabili-
ties and ethical challenges. First, AI models, espe-
cially those based on ML and DL, can be deceived
by adversarial examples that cause misclassifica-
tion or evasion of detection. Attackers can manip-
ulate data to bypass AI-driven security systems or
even poison training data to degrade model perfor-
mance [13]. These systems also require large, high-
quality, and accurately labeled datasets to work ef-
fectively. In cybersecurity, such datasets are often
scarce, proprietary, imbalanced (more normal than
malicious data), or not representative of real-world
threats [13].
Another key limitation is lack of explainability
and transparency.
DL systems, operate as black
boxes, making it difficult for security analysts to un-
derstand or trust their decisions. This can compli-
cate incident response, forensic analysis, and com-
pliance with regulatory requirements.
Even with
explainable AI (XAI) techniques, providing reli-
able, interpretable explanations in real-time, high-
risk scenarios remain a challenge [14], [15].
There are also privacy and data protection risks.
Using large datasets for training and operating AI
systems can conflict with privacy regulations like
GDPR (General Data Protection Regulation) and
increase the risk of sensitive data exposure.
AI
models may accidentally leak information about in-
dividuals or organizations, raising significant pri-
vacy and legal concerns [15], [16].
3
Data Sources
Understanding what sources can support CTI for AI
is an important first step, because the quality of the
intelligence depends directly on the quality of the
data feeding it. Different organizations and research
groups have already created useful frameworks, tax-
onomies, and incident databases that help map out
how AI can be misused, where failures occur, and
what kinds of risks are most common.
Frameworks like AVID, SAIF, and ENISA
mainly focus on good cybersecurity practices in AI
systems, offering guidance on how to prevent or re-
duce risks. MITRE ATLAS, in contrast, goes fur-
ther and can be integrated directly into broader
CTI feeds, making it more suitable for operational
use.
Incident-oriented sources are also essential,
because they document real-world cases of misuse
and system failure [17], giving analysts concrete ex-
amples rather than only theoretical risk categories.
The CSET AI Harm Taxonomy is especially rele-
vant for CTI for AI, as it provides a structured way
to describe the consequences of AI incidents and
the sectors they affect.
For CTI work, such tax-
onomies help prioritize threats based on impact and
give a clearer picture of where protective measures
are most urgently needed.
3.1
Vulnerability-Oriented Sources
Vulnerability-oriented sources focus on weaknesses
in AI/ML systems that attackers can exploit. They
play a similar role to CVE and CWE-style reposito-
ries in traditional cybersecurity, offering structured
knowledge about technical issues that create risk.
In CTI for AI, these sources are valuable because
they help analysts identify, categorize, and track the
root causes of vulnerabilities across the AI lifecycle.
Below, we describe several representative examples.
AI Vulnerability Database (AVID)
AVID is
an open-source database developed by the AI Risk
and Vulnerability Alliance [18]. It collects AI/ML
vulnerabilities submitted by contributors, each as-
signed a unique ID and short description.
AVID
focuses on development-phase vulnerabilities and
does not currently cover deployment or operational
aspects. It can serve as a structured repository that
CTI platforms may integrate into their feeds.
The database distinguishes between two classes:
vulnerabilities and reports. A single vulnerability
may have multiple associated reports. Public en-
tries visible on the website include: 13 vulnerability
entries in 2022 (V001–V013) and 27 entries in 2023
(V001–V027), totaling 40 vulnerabilities. As for re-
ports (concrete incidents), the site lists: 5 reports
in 2022, 3 in 2023, and 2 in 2025. This results in
10 visible reports, indicating that activity is present
but not continuous. The existence of reports from
2025 shows that the project remains active. An ex-
ample of a report is shown in Figure 1.
AVID also provides a taxonomy that introduces
a standardized language for describing AI risks. At
a high level, it includes two complementary views:
• Effect view – used mainly by auditors eval-
uating risks of AI artifacts (datasets, models,
systems),
• Lifecycle view – for developers examining
risks at each step of the ML workflow.
The effect view comprises three domains (SEP):
• Security – vulnerabilities and attacks,
• Ethics – bias and privacy issues,
• Performance – model and system quality.
Figure 2 shows the structure of the AVID tax-
onomy matrix. The lifecycle view is based on the
CRISP-DM methodology and includes six stages:
• L01 - Business Understanding,
• L02 - Data Understanding,
• L03 - Data Preparation,
3




![image](sources/images/krawczyk-2026-cti-for-ai-systems/p4_img1.png)

Figure 1: An example report entry in the AVID database [19].
• L04 - Model Development,
• L05 - Evaluation,
• L06 - Deployment.
Figure 3 shows how risks can be represented in
three dimensions: domain, category, subcategory,
and lifecycle stage.
OWASP AI Security and Privacy Guide
The OWASP AI Security and Privacy Guide lists
common attack types targeting AI systems, includ-
ing poisoning, adversarial examples, and privacy
leakage [20]. It extends OWASP’s long tradition of
cataloging security risks (such as OWASP Top 10).
While it is not a vulnerability database, it provides
a structured overview of attack techniques and rec-
ommended controls. In a CTI context, it works best
when combined with repositories such as AVID or
frameworks like MITRE ATLAS.
European Union Agency for Cybersecurity
(ENISA)
ENISA publishes guidelines and re-
ports on AI security,
including the Multilayer
Framework for Good Cybersecurity Practices in
AI [21]. This framework outlines the AI lifecycle
and related security measures.
Although ENISA
does not maintain a vulnerability repository, its
materials help structure CTI knowledge by linking
risks, controls, and lifecycle stages.
Secure
AI
Framework
(SAIF)
SAIF
is
Google’s security framework providing guidelines
for safe development and deployment of AI sys-
tems [22].
It functions primarily as a best-
practices playbook rather than a vulnerability tax-
onomy. Nevertheless, it can complement other CTI
sources by offering standardized recommendations
that support risk categorization.
Overall, OWASP, ENISA, and SAIF provide
valuable structure for organizing knowledge about
AI risks, but they do not function as standalone
vulnerability repositories. Their contribution lies in
standardizing terminology, mapping threats to life-
cycle phases, and guiding secure development prac-
tices. In contrast, AVID offers a more direct source
of vulnerability data that CTI platforms can incor-
porate.
3.2
Incident-Oriented Sources
Incident-oriented sources document real-world cases
of AI failures,
misuses,
and harms.
Unlike
vulnerability-focused repositories, which describe
potential weaknesses, incident databases capture
what actually happened, who was affected, and
what the consequences were.
For CTI for AI,
these sources provide empirical evidence that com-
plements raw vulnerability data, helping analysts
understand how issues materialize in practice. The
most noteworthy examples are described below.
4




![image](sources/images/krawczyk-2026-cti-for-ai-systems/p5_img2.png)

Figure 2: The AVID taxonomy matrix [18].

![image](sources/images/krawczyk-2026-cti-for-ai-systems/p5_img3.png)

Figure 3: SEP and Lifecycle views of the AVID taxonomy represent different sections of the space of
potential risks in an AI development workflow [18].
AI Incident Database (AIID)
The AI Inci-
dent Database is a large, community-driven repos-
itory maintained by the Responsible AI Collabora-
tive [19]. Its goal is to support incident avoidance,
analysis, and mitigation by collecting detailed re-
ports of AI-related harms. The database contains
more than 1000 archived incident reports [17], and
it supports full-text search and faceted filtering to
enable research and monitoring.
Reported cases include:
an autonomous car
killing a pedestrian, a trading algorithm causing a
financial “flash crash”, or a facial recognition sys-
tem contributing to a wrongful arrest. As of March
2026 the MIT AI Incident Tracker (Figure 4) claims
AIID contains a total of 5499 processed reports, cor-
responding to 1366 distinct incidents, since many
incidents have multiple reports.
In practical terms, AIID maintains two layers
of attributes. The first consists of core metadata
fields, which are consistently present for each entry.
These include:
• Incident/Issue ID – an AI incident is an
alleged harm or near-harm involving an AI
system; an AI issue is a potential or antici-
pated harm; incident variants represent simi-
lar events involving the same systems,
• Title – short incident name,
• Description – summary of what occurred,
• Date – approximate date of occurrence,
• Alleged deployer – the organization oper-
ating the implicated AI system,
• Harmed or nearly harmed parties,
• Implicated system.
These fields form the backbone of the database
and are reliably populated in nearly all records.
Figure 5 shows an example of core metadata.
Beyond
core
attributes,
AIID
supports
taxonomy-driven classifications using several ex-
isting frameworks (e.g., the CSET AI Harm Taxon-
omy, GMF taxonomy). These fields allow incidents
to be categorized by harm type, affected sector,
attacker intent, failure mode, or lifecycle phase
(Figure 6). However, these attributes are optional
and, in practice, are populated inconsistently. Most
reports contain only core metadata, with taxonomy-
driven annotations remaining underused.
5




![image](sources/images/krawczyk-2026-cti-for-ai-systems/p6_img4.png)

Figure 4: Number of reports in AI Incident Database (AIID) in 2015-2025 [23].
CSET AI Harm Taxonomy
The CSET AI
Harm Taxonomy provides a structured way to clas-
sify harms involving AI systems [19].
It charac-
terizes affected sectors, types of harm, involved
entities, and contextual factors.
According to
the CSET incident database, the most affected
domains include information and communication,
transportation, law enforcement, public administra-
tion, health and social work, and retail [24].
The taxonomy defines four necessary elements
that must be present for an event to qualify as an
AI harm:
• An entity that experienced the harm,
• The harmful event or issue,
• The implicated AI system,
• A direct link to the behavior.
All four criteria must be satisfied. This defini-
tion helps distinguish AI-related harms from unre-
lated failures or general operational issues.
Goals, Methods, and Failures (GMF)
The
GMF taxonomy is designed for analyzing the tech-
nical causes of real-world AI failures [19]. It links:
• System goals (e.g., identity recognition),
• AI methods and technologies (e.g., trans-
former architectures),
• Failure causes (e.g., concept drift, insuffi-
cient training data),
while also allowing annotators to add confidence
levels and cite supporting text snippets.
The taxonomy is optimized for situations where
incident reports are short, incomplete, or noisy,
making it suitable for large-scale annotation efforts.
Its purpose is to connect harms to system inten-
tions, tie failure causes to technical components,
and support consistent expert review.
Figure 7 illustrates the GMF annotation pro-
cess, while Figure 8 shows an example application
to an AIID incident.
3.3
Adversary-Oriented Sources
A third category of sources focuses on adver-
saries and their tactics, techniques, and procedures
(TTPs). These frameworks describe how attackers
operate against AI systems in practice, similar to
how MITRE ATT&CK describes attacker behavior
in classical cybersecurity. For CTI, TTP-oriented
sources provide actionable knowledge on attacker
strategies, motivations, and potential impact. Rep-
resentative examples are described below.
MITRE ATLAS (Adversarial Threat Land-
scape
for
Artificial
Intelligence
Systems)
MITRE ATLAS is a comprehensive framework that
maps attack vectors against AI/ML systems [25]. It
is analogous to MITRE ATT&CK for traditional IT
systems but focuses on AI-specific adversarial tech-
niques such as model poisoning, evasion attacks,
and adversarial examples.
MITRE ATLAS serves as a CTI knowledge base
for AI, offering structured descriptions of attacker
tactics, techniques, and procedures. This makes it
directly relevant for analysts seeking to understand
patterns of AI exploitation and anticipate potential
threats.
Attacking
Artificial
Intelligence
Report
Marcus Comiter’s report classifies various attack
types and vectors against AI [26]. It introduces con-
ceptual axes such as human-perceivable vs. invis-
6




![image](sources/images/krawczyk-2026-cti-for-ai-systems/p7_img5.png)

Figure 5: Example core metadata fields in AIID [19].
ible attacks and physical vs. digital attacks. The
report also links attacker intent to potential con-
sequences, providing insight into motivations and
severity. While it lacks the structured format for
automated vulnerability tracking, it can enrich CTI
with qualitative understanding of attacker behavior
and expected impact.
Complementary frameworks
In addition to
MITRE ATLAS and Comiter’s report, several other
initiatives provide general guidance for secure AI
development and deployment. Notable examples in-
clude ENISA’s AI security guidelines and Google’s
SAIF framework. While these sources do not di-
rectly provide TTP data, they serve as useful refer-
ences for structuring CTI, mapping controls to AI
lifecycle stages, and contextualizing adversary be-
haviors.
4
AI-Specific Vulnerabilities
The rapid growth of AI systems brings not only new
opportunities but also new risks. While traditional
IT systems benefit from established frameworks for
vulnerabilities, such as CVE, CWE, or CVSS-AI
lacks a widely accepted standard for categorizing
its weaknesses. Existing efforts, like AVID, MITRE
ATLAS, Google’s Secure AI Framework, OWASP,
and ENISA, provide some guidance, but they re-
main incomplete or inconsistent [27].
High-risk AI systems should be designed to re-
sist attempts by unauthorized parties to manipu-
late their behavior, outputs, or performance. Pro-
tective measures may include preventing or detect-
ing attacks on training data (data poisoning), pre-
trained models (model poisoning), carefully crafted
inputs that mislead the AI (adversarial exam-
ples), breaches of confidentiality, or other model
flaws [24].
In general, a vulnerability in an AI system is
any weakness that could be exploited to produce
undesired effects, such as incorrect predictions, pri-
vacy leaks, degraded performance, or manipulation
during deployment.
However, simply listing vul-
nerabilities does not explain how they are actually
exploited. Here, CTI for AI plays a key role, as it
combines research on potential weaknesses with ev-
idence from real-world incidents, attacker TTPs, as
well as operational context. This approach trans-
forms static vulnerability lists into actionable intel-
ligence that can support risk management, detec-
tion, and response.
Pispa et al. [27] propose a structured taxonomy
for AI vulnerabilities with three main goals: stan-
dardize classification, identifying where in the AI
lifecycle a vulnerability occurs; describe affected as-
pects of trustworthy AI, such as accuracy, fairness,
7




![image](sources/images/krawczyk-2026-cti-for-ai-systems/p8_img6.png)

Figure 6: Example GMF taxonomy classification for a specific incident [19].
privacy, robustness, safety, and reliability; and as-
sess potential impact using a seven-level scale, from
minimal effect to full attacker control.
The taxonomy organizes knowledge in three
steps:
1. Vulnerability location:
• Development phase (e.g., malicious li-
braries or hardware),
• Training phase (e.g., poisoned datasets,
attacks in federated learning),
• Deployment phase (e.g., adversarial in-
puts, model inversion attacks).
2. Affected attributes of trustworthy AI:
accuracy,
fairness,
privacy,
reliability,
re-
silience, robustness, and safety.
3. Impact scale (7 levels): from normal func-
tioning, through degraded or unintended be-
havior, to full attacker-directed actions be-
yond system limits.
For instance, a physical-world attack on traffic
signs would be categorized as a deployment-phase
vulnerability, affecting both accuracy and safety by
misleading image classifiers in autonomous vehicles.
This taxonomy provides a clear framework to
analyze and compare AI vulnerabilities and can
serve as a foundation for CTI in AI, which should
document not only actual incidents but also poten-
tial weaknesses and ways they might be misused.
5
Collecting AI Incidents
Understanding and organizing AI incidents is es-
sential for any work that aims to build a CTI
for AI knowledge base.
Many organizations al-
ready track such cases and maintain public reposi-
tories (e.g., the AI Incident Database), which shows
how important systematic documentation has be-
come [24].
For CTI, these resources help iden-
tify patterns, weak points in deployed systems, and
broader trends in how AI fails or is intentionally
misused.
A key contribution to this space is the CSET
AI Harm Taxonomy, which provides a clear struc-
ture for describing the impact of AI-related inci-
dents and the sectors they affect. From a CTI per-
spective, such taxonomies allow analysts to group
incidents by their consequences and relate them to
specific risk domains such as healthcare, finance, or
transportation.
This makes it easier to prioritize
threats and connect technical vulnerabilities with
real-world implications [24].
Another useful framework is the GMF taxon-
omy.
It describes incidents by focusing on what
the adversary intended to achieve, how the attack
was executed, and what type of failure occurred.
For CTI, this perspective aligns well with adversary
modeling.
It supports mapping observed model
weaknesses (e.g., deepfake generation or failures in
autonomous driving) to plausible attacker strategies
and affected technologies [24].
In cybersecurity, attacks are often analyzed
through the classic triad of assets, vulnerabilities,
and threats.
Although originally defined for dig-
ital systems (data, software, storage, network),
8




![image](sources/images/krawczyk-2026-cti-for-ai-systems/p9_img7.png)

Figure 7: GMF annotation process diagram [19].
this structure is still helpful when adapted to AI.
For example, AI-specific assets include training
data, model parameters, or model APIs; AI-specific
vulnerabilities cover poisoning, evasion, or back-
door insertion.
Reinterpreting this triad allows
CTI for AI to systematically categorize AI-focused
threats using concepts already familiar in cyberse-
curity [24].
Public databases describing TTPs,
such as
MITRE ATT&CK, have also been extended to AI.
MITRE’s ATLAS framework documents adversar-
ial behaviors targeting ML systems and mirrors the
structure of classic cyber attacks, but adds ML-
specific adaptations [24]. The phases relevant for
AI/ML systems typically include:
• Pre-attack: reconnaissance and collection of
information about ML artifacts, models, or
integrated services,
• Attack phase: gaining initial access to a lo-
cal or cloud-based ML system, often through
APIs or a product that embeds the target
model,
• Execution:
running malicious code inside
ML-related software or artifacts,
• Persistence: maintaining long-term access,
often by inserting a model-level backdoor that
can be activated by specific triggers,
• Privilege escalation: especially relevant for
systems using LLMs, where attackers may in-
crease their permissions using prompt attacks,
plugin compromise, or jailbreak attempts,
• Defense evasion: bypassing ML-based de-
tection mechanisms,
• Credential access: stealing credentials via
keylogging or other extraction methods,
• Model-specific
attacks:
such as proxy
model creation, data poisoning, or adversarial
examples,
• Exfiltration: stealing models, datasets, or
system information,
• Impact:
degrading system integrity, avail-
ability, or manipulating ML outputs.
These phases highlight which elements of AI
systems can be targeted and support defining AI-
specific incident categories and indicators of com-
promise, such as model manipulation, training data
interference, or LLM jailbreak attempts. A com-
mon way to classify adversarial attacks is by the
attacker’s knowledge [24]:
• White-box: full access to the model, includ-
ing parameters and training data,
• Black-box: no direct access, only observable
input–output behavior,
• Grey-box / transferability: attacks rely-
ing on similarities between different models.
Another dimension is the attacker’s goal:
• Untargeted: forcing the model to produce
any incorrect output,
• Targeted:
pushing the model toward one
specific wrong prediction.
9




![image](sources/images/krawczyk-2026-cti-for-ai-systems/p10_img8.png)

Figure 8: Application of the annotation process for the real-world AIID incident (ID 72) [19].
Attacks also differ by where they occur in
the ML pipeline:
• Training: data poisoning,
• Model: inserting backdoors,
• Deployment: inference-time evasion or ma-
nipulation.
They can also be grouped by model type:
• Deep models (e.g., transformers), which are
targeted most often due to their widespread
use,
• Other
algorithms (e.g., SVMs, GNNs),
which may be exploited differently.
Finally, attacks can be distinguished by their
attack surface:
• Digital: direct manipulation of input data or
model components,
• Physical: altering real-world objects or envi-
ronments (e.g., stickers on road signs, adver-
sarial clothing, audio perturbations).
Together, these taxonomies help structure AI in-
cident reporting and guide the creation of a CTI
for AI knowledge base by indicating which systems,
models, or operational settings are most vulnerable.
6
Prompt Injection Datasets
Prompt injection represents a major security con-
cern for large language models (LLMs).
In such
attacks, malicious instructions are embedded into
prompts or external inputs to manipulate model
outputs [28]. As LLMs are increasingly deployed in
real-world applications, understanding prompt in-
jection (its nature, impact, and possible defenses)
has become essential for AI safety and security.
Several datasets have been created to study and
benchmark prompt injection attacks. Based on the
evaluation by HiddenLayer [29], they can be cate-
gorized as recommended, use with caution, or not
recommended.
Recommended
datasets
These datasets are
considered high quality and suitable for research
and evaluation:
• Qualifire
Prompt
Injections
Bench-
mark1: 5000 samples, with 60% benign and
40% jailbreak examples, mostly in English,
mixing prompt injections and roleplay-centric
jailbreaks,
• Prompt Injection Attack Dataset2: 3750
mostly English samples combining benign in-
puts with various prompt injection strategies,
• Multilingual Prompt Injections3: short,
simple injections in multiple European lan-
guages (English, French, German, Spanish,
1https://huggingface.co/datasets/qualifire/prompt-injections-benchmark
2https://huggingface.co/datasets/xxz224/prompt-injection-attack-dataset
3https://huggingface.co/datasets/yanismiraoui/prompt injections
4https://huggingface.co/datasets/jayavibhav/prompt-injection-safety
10



Italian, Portuguese, Romanian), useful for
evaluating multilingual robustness,
• Prompt Injection Safety Dataset4: 50k
training and 10k test samples, labels: 0 = be-
nign, 1 = prompt injection, 2 = direct request
for harmful behavior.
Use with caution
These datasets may be useful
but have limitations, such as label quality, size, or
coverage:
• Jayavibhav Prompt Injection5: large and
evenly distributed labels: 0 = benign, 1 =
injection, but some benign samples may still
trigger toxic outputs, subsampling (10k per
class) is recommended,
• Deepset Prompt Injections6: 662 samples
in English, German, and French, focused on
politically biased prompts, useful for evaluat-
ing political guardrails.
Not
recommended
Datasets with significant
limitations, such as missing labels or poor quality,
which reduce their usefulness for research at the mo-
ment, but could be improved in the future:
• HackAPrompt7:
602k multilingual sam-
ples, no labels, narrow attack focus,
• Prompt
Injection
Password/Secret8:
limited scope, likely created by a single par-
ticipant,
• Prompt Injection Dataset9: label incon-
sistencies, unreliable as benign,
• Prompt Injection Cleaned Dataset10:
repackaged HackAPrompt with label noise at
higher difficulty levels,
• MOSSCAP
Prompt
Injection11:
un-
labeled CTF dataset with repetitive and
narrow-scope attacks.
Other collections
Some additional datasets are
worth mentioning, even if not categorized:
• Prompt Injection in the Wild12: a compi-
lation of prompt injection datasets from Hug-
gingFace and Kaggle,
• Prompt Injection Malignant13: 199 jail-
break prompts (70 original, 129 augmented
paraphrases),
• Malicious Prompts14: dataset without doc-
umentation or labels.
7
Malicious Model Files
The growing adoption of ML in various applications
has introduced new security challenges. One such
challenge stems from the distribution of malicious
models or repositories, which may contain poisoned
training data, hidden backdoors, or other vulnera-
bilities. These threats are particularly concerning
because they can evade standard detection meth-
ods and can be shared widely through public model
repositories and leaderboards [30].
To illustrate this issue, we present examples of
indicators of compromise (IoCs) related to mali-
cious ML models and repositories. These IoCs were
collected from investigations conducted by Revers-
ingLabs into software supply chain attacks target-
ing machine learning resources [31].
Table 1 lists some known malicious files hosted
on Hugging Face. Researchers and practitioners can
use similar tables as a reference when defining IoCs
for this category of threats.
Some repositories hosting these malicious mod-
els include [31]:
• glockr1/ballr7
• who-r-u0000/OOOOOOOOOOOOOOOO-
OOOOOOOOOOOOOOOOOOOOOOO-
OOOOOOOOOOO
In addition to files and repositories, associated
IP address have been identified [31]:
• 107.173.7.141
Additionally,
Figure 9 shows example IoCs
based on NSFOCUS Threat Intelligence [32]. In one
analysis, researchers examined nine malicious mod-
els from the Hugging Face user Star23 and further
investigated four IPs identified through NSFOCUS
Threat Intelligence (NTI). This figure demonstrates
how threat intelligence can support the identifica-
tion of malicious ML artifacts and help define IoCs
in practice.
5https://huggingface.co/datasets/jayavibhav/prompt-injection
6https://huggingface.co/datasets/deepset/prompt-injections
7https://huggingface.co/datasets/hackaprompt/hackaprompt-dataset
8https://huggingface.co/datasets/cgoosen/prompt injection password or secret
9https://huggingface.co/datasets/geekyrakshit/prompt-injection-dataset
10https://huggingface.co/datasets/imoxto/prompt injection cleaned dataset
11https://huggingface.co/datasets/Lakera/mosscap prompt injection
12https://www.kaggle.com/datasets/arielzilber/prompt-injection-in-the-wild
13https://www.kaggle.com/datasets/marycamilainfo/prompt-injection-malignant
14https://huggingface.co/datasets/ahsanayub/malicious-prompts
11



Table 1: Example malicious model files found in Hugging Face.
Model
File type
SHA1
glockr1/ballr7
PyTorch
1733506c584dd6801accf7f58dc92a4a1285db1f
glockr1/ballr7
Pickle
79601f536b1b351c695507bf37236139f42201b0
who-r-u0000/0000000000000000000000000000000000000
PyTorch
0dcc38fc90eca38810805bb03b9f6bb44945bbc0
who-r-u0000/0000000000000000000000000000000000000
Pickle
85c898c5db096635a21a9e8b5be0a58648205b47

![image](sources/images/krawczyk-2026-cti-for-ai-systems/p12_img9.png)

Figure 9: Example IoCs based on NSFOCUS Threat Intelligence [32].
8
Indicators of Compromise
When building a CTI knowledge base for AI sys-
tems, defining IoCs is a crucial first step. IoCs are
pieces of evidence that suggest a system or dataset
might be compromised or malicious.
In cyberse-
curity, these often include file hashes, IP addresses,
and other malware signatures. In the context of AI,
some IoCs may overlap with existing ones, but there
is also a need to define new indicators specific to AI
assets, such as suspicious model weights, unusual
dataset patterns, or modified training scripts.
Once IoCs are defined, it is important to es-
tablish methods for measuring similarity between
collected IoCs and potentially malicious AI models
or datasets. This similarity measurement helps in
detecting reused or modified AI assets and in effi-
ciently querying the CTI knowledge base.
One approach is deep hashing, which transforms
complex AI assets into compact binary fingerprints
while preserving semantic similarity.
Deep hash-
ing allows fast comparison of models or datasets
without processing the full data, which is critical
for large-scale knowledge bases. Features such as
model weights, architecture patterns, tokenizer fin-
gerprints, or dataset embeddings are first extracted,
then encoded into binary hash codes. Similar as-
sets produce similar hash codes, and the Ham-
ming distance can be used to quantify similarity.
This method enables fast retrieval of exact or near
matches, even for previously unseen assets, and
handles small changes [33].
Another class of methods comes from mal-
ware analysis and includes similarity hashing al-
gorithms like TLSH and LZJD. These techniques
generate similarity digests that summarize files or
datasets, allowing fast computation of similarity
scores.
TLSH, for example, produces short di-
gests that retain meaningful information while be-
ing efficient in storage and computation. These ap-
proaches have been successfully used for clustering
and searching large datasets, and similar techniques
can be adapted for AI assets to enable scalable de-
tection of suspicious models or datasets [34], [35].
To improve robustness and semantic fidelity,
newer methods such as semantic consistency hash-
ing (SCH) have been proposed. SCH converts local
similarity structures into probability distributions
to better preserve global semantic information and
uses transformation consistency learning to main-
tain stable hash codes under perturbations. Experi-
ments show that SCH outperforms previous hashing
methods in maintaining semantic similarity while
being robust to changes in the input data [36].
Fuzzy hashing is another technique that calcu-
lates similarity between files or datasets based on
partial matches, rather than exact equality. Com-
bining results from different fuzzy hashing methods
can improve detection rates, providing an efficient
way to identify modified or polymorphic AI assets
without fully processing them [37].
Overall, these similarity measures are essential
for AI CTI. They enable fast and scalable search-
ing, clustering, and detection of suspicious assets
in large knowledge bases. Developing appropriate
hashing or similarity algorithms for AI-specific IoCs
will be key to building effective AI threat intelli-
gence systems.
9
Conclusion
This review examined how cyber threat intelligence
practices need to adapt when applied to AI systems.
We analyzed existing frameworks, taxonomies, and
data sources, and identified key requirements for
12



building effective CTI for AI. Below, we directly
address each research question posed at the begin-
ning of this study.
RQ1: What are the differences between clas-
sical CTI and CTI for AI?
Classical CTI
focuses on conventional IT assets like networks,
servers, and software.
CTI for AI must account
for unique assets including training datasets, model
weights, model architectures, APIs, and inference
pipelines. While traditional CTI tracks vulnerabil-
ities like buffer overflows and SQL injection, CTI
for AI must address AI-specific weaknesses such as
data poisoning, model backdoors, adversarial exam-
ples, model inversion attacks, and prompt injection.
Moreover, traditional attacks follow patterns docu-
mented in frameworks like MITRE ATT&CK. AI
attacks introduce new phases specific to the ML
lifecycle: reconnaissance of ML artifacts, poison-
ing during training, backdoor insertion in models,
evasion during inference, and model extraction or
theft.
RQ2: What sources can be used to build a
CTI for AI knowledge base?
How reliable
are these data sources?
We identified three
main categories of sources for building CTI for AI
knowledge bases: vulnerability-oriented, incident-
oriented, and adversary-oriented sources. There are
also specialized datasets, such as prompt injection
datasets and malicious model repositories.
Their
quality varies: prompt injection datasets have vari-
able quality, with recommended datasets (Qualifire,
Prompt Injection Attack Dataset) being reliable for
research, while many others have limitations in la-
beling or scope. Malicious model repositories have
few documented cases (e.g., ReversingLabs investi-
gations, NSFOCUS reports); they are reliable for
some of the cases, but their coverage is very lim-
ited. Overall, current sources provide a foundation
but remain incomplete. MITRE ATLAS and AIID
are the most mature and reliable.
AVID shows
promise but needs more contributors. Many spe-
cialized datasets have quality issues.
RQ3: How would a CTI for AI knowledge
base benefit tools designed to protect AI?
A CTI for AI knowledge would provide impor-
tant support for AI protection tools.
By storing
signatures and patterns of known malicious mod-
els, datasets, and attack techniques, tools could
scan AI systems before deployment or investigate
suspicious behavior by comparing it to past inci-
dents.
This helps identify likely causes and ap-
ply proven fixes, as enabled by resources like the
AIID incident database and CSET taxonomy. Tools
could also find which vulnerabilities affect specific
model types, frameworks, or deployment scenarios,
using repositories such as AVID. Encoding adver-
sary TTPs from MITRE ATLAS allows monitoring
for reconnaissance, data poisoning, or evasion at-
tempts, similar to how endpoint detection and re-
sponse (EDR) tools use ATT&CK. With deep hash-
ing and fuzzy matching techniques, even previously
unseen malicious models can be detected if they are
similar to known threats. Finally, structured tax-
onomies like AVID, CSET, and GMF help auto-
matically sort incidents, assess severity, and guide
response actions. In essence, a CTI for AI knowl-
edge works for AI security tools much like tradi-
tional threat intelligence feeds do for network secu-
rity, giving the context needed to detect, analyze,
and respond to threats effectively.
RQ4: How to measure the similarity between
collected IoCs and potentially malicious AI
artifacts? How to effectively query the CTI
for AI knowledge base?
We identified several
approaches for measuring similarity and querying
AI-specific IoCs.
Deep hashing and fuzzy hash-
ing transform models, datasets, and features into
compact codes, enabling fast comparison even for
modified assets. Techniques from malware analy-
sis, such as TLSH or LZJD, can also be adapted for
AI assets. SCH preserves global semantic informa-
tion, improving similarity detection. Practical CTI
queries can combine exact lookups (hashes, reposi-
tory names) with similarity searches using deep or
fuzzy hashes.
Features from model architectures,
weights, or dataset profiles can be hashed for fast
retrieval, and similarity scores from multiple meth-
ods can be aggregated to improve accuracy. Query
strategies may include behavioral searches, contex-
tual filters, and temporal analysis to track emerg-
ing threats. The main challenge is balancing speed
with detection accuracy. Efficient hashing enables
real-time scanning, while richer similarity measures
support deeper investigation, making a CTI system
versatile and effective.
10
Future Work
Future research may focus on defining novel IoCs
tailored for AI systems.
Current CTI methods
mostly focus on traditional software and networks,
but AI frameworks, models, and datasets have
unique behaviors and vulnerabilities. It may also be
worth investigating what signs indicate that an AI
model has been tampered with. This could include
unusual model outputs, poisoned training data, or
suspicious model updates. Another direction is to
design and test these AI-specific IoCs in practice,
and to create a framework that helps security teams
monitor and respond to AI threats.
13



References
[1]
F. Aldauiji, O. Batarfi, and M. Bayousef, “Utilizing cyber threat hunting techniques to find ran-
somware attacks: A survey of the state of the art,” IEEE Access, vol. 10, pp. 61 695–61 706, 2022.
[2]
F. Sufi and M. Alsulami, “Quantifying temporal dynamics in global cyber threats: A gpt-driven
framework for risk forecasting and strategic intelligence,” Mathematics, vol. 13, no. 10, p. 1670,
2025.
[3]
F. Sufi, “A new time series dataset for cyber-threat correlation, regression and neural-network-based
forecasting,” Information, vol. 15, no. 4, p. 199, 2024.
[4]
A. K. D. Kazim and N. R. Shanshul, “The impact of cyber-attacks on companies and organisations
in developed countries,” Edelweiss Applied Science and Technology, vol. 8, no. 6, pp. 9245–9252,
2024.
[5]
P. Alaeifar, S. Pal, Z. Jadidi, M. Hussain, and E. Foo, “Current approaches and future directions
for cyber threat intelligence sharing: A survey,” Journal of Information Security and Applications,
vol. 83, p. 103 786, 2024.
[6]
A. T. Olutimehin, A. J. Ajayi, O. C. Metibemu, A. Y. Balogun, T. O. Oladoyinbo, and O. O. Olaniyi,
“Adversarial threats to ai-driven systems: Exploring the attack surface of machine learning models
and countermeasures,” Available at SSRN 5137026, 2025.
[7]
L. N. Tidjon and F. Khomh, “Threat assessment in machine learning based systems,” arXiv preprint
arXiv:2207.00091, 2022.
[8]
C. Okoli and K. Schabram, “A guide to conducting a systematic literature review of information
systems research,” Working Papers on Information Systems, 2015.
[9]
A. Arif, M. I. Khan, and A. R. A. Khan, “An overview of cyber threats generated by ai,” Interna-
tional Journal of Multidisciplinary Sciences and Arts, vol. 3, no. 4, pp. 67–76, 2024.
[10]
S. Ankalaki, A. A. Rajesh, M. Pallavi, G. S. Hukkeri, T. Jan, and G. R. Naik, “Cyber attack
prediction: From traditional machine learning to generative artificial intelligence,” IEEE Access,
2025.
[11]
J. Sivakumar, N. R. Salman, F. R. Salman, H. R. Salimova, and E. Ghimire, “Ai-driven cyber threat
detection: Enhancing security through intelligent engineering systems,” Journal of Information
Systems Engineering and Management, vol. 10, no. 19, pp. 790–798, 2025.
[12]
S. Dasgupta, B. V. Yelikar, S. Naredla, R. K. Ibrahim, M. B. Alazzam, et al., “Ai-powered cyber-
security: Identifying threats in digital banking,” in 2023 3rd international conference on advance
computing and innovative technologies in engineering (ICACITE), IEEE, 2023, pp. 2614–2619.
[13]
M. Ozkan-Okay et al., “A comprehensive survey: Evaluating the efficiency of artificial intelligence
and machine learning techniques on cyber security solutions,” IEEe Access, vol. 12, pp. 12 229–
12 256, 2024.
[14]
N. Capuano, G. Fenza, V. Loia, and C. Stanzione, “Explainable artificial intelligence in cybersecu-
rity: A survey,” Ieee Access, vol. 10, pp. 93 575–93 600, 2022.
[15]
G. Rjoub et al., “A survey on explainable artificial intelligence for cybersecurity,” IEEE Transac-
tions on Network and Service Management, vol. 20, no. 4, pp. 5115–5140, 2023.
[16]
M. Gupta, C. Akiri, K. Aryal, E. Parker, and L. Praharaj, “From chatgpt to threatgpt: Impact of
generative ai in cybersecurity and privacy,” IEEE Access, vol. 11, pp. 80 218–80 245, 2023.
[17]
S. McGregor, “Preventing repeated real world ai failures by cataloging incidents: The ai inci-
dent database,” in Proceedings of the AAAI Conference on Artificial Intelligence, vol. 35, 2021,
pp. 15 458–15 463.
[18]
AI Risk and Vulnerability Alliance. “Ai vulnerability database (avid),” Accessed: Mar. 4, 2026.
[Online]. Available: https://avidml.org
[19]
Responsible AI Collaborative. “Artificial intelligence incident database,” Accessed: Mar. 4, 2026.
[Online]. Available: https://incidentdatabase.ai
[20]
OWASP Foundation. “Owasp ai exchange,” Accessed: Mar. 4, 2026. [Online]. Available: https:
//owaspai.org
14



[21]
ENISA. “Multilayer framework for good cybersecurity practices for ai,” Accessed: Mar. 4, 2026.
[Online]. Available: https://www.enisa.europa.eu/sites/default/files/publications/
Multilayer%20Framework%20for%20Good%20Cybersecurity%20Practices%20for%20AI.pdf
[22]
Google. “Secure ai framework (saif),” Accessed: Mar. 4, 2026. [Online]. Available: https://www.
saif.google
[23]
MIT AI Risk Initiative. “Mit ai incident tracker,” Accessed: Mar. 4, 2026. [Online]. Available:
https://airisk.mit.edu/ai-incident-tracker
[24]
A. Viureanu and B. Ionescu, “Ai vulnerabilities,” Database, vol. 2, p. 3, 2024.
[25]
Mitre Corporation. “Mitre atlas (adversarial threat landscape for artificial intelligence systems),”
Accessed: Mar. 4, 2026. [Online]. Available: https://atlas.mitre.org/matrices/ATLAS
[26]
M. Comiter, “Attacking artificial intelligence,” Belfer Center Paper, vol. 8, pp. 2019–08, 2019.
[27]
A. Pispa and K. Halunen, “A comprehensive artificial intelligence vulnerability taxonomy,” in Pro-
ceedings of the 23rd European Conference on Cyber Warfare and Security, ECCWS 2024, Academic
Conferences International Limited, 2024.
[28]
K. Greshake, S. Abdelnabi, S. Mishra, C. Endres, T. Holz, and M. Fritz, “Not what you’ve signed
up for: Compromising real-world llm-integrated applications with indirect prompt injection,” in
Proceedings of the 16th ACM workshop on artificial intelligence and security, 2023, pp. 79–90.
[29]
K. Stangl and A. Davis. “Evaluating prompt injection datasets,” Accessed: Mar. 4, 2026. [On-
line]. Available: https://www.hiddenlayer.com/research/evaluating-prompt-injection-
datasets
[30]
B. Chen et al., “Detecting backdoor attacks on deep neural networks by activation clustering,”
arXiv preprint arXiv:1811.03728, 2018.
[31]
K. Zanki. “Malicious ml models discovered on hugging face platform,” Accessed: Oct. 6, 2025.
[Online]. Available: https://www.reversinglabs.com/blog/rl- identifies- malware- ml-
model-hosted-on-hugging-face
[32]
NSFOCUS, Inc. “Ai supply chain security: Hugging face malicious ml models,” Accessed: Oct. 7,
2025. [Online]. Available: https://nsfocusglobal.com/ai-supply-chain-security-hugging-
face-malicious-ml-models
[33]
X. Luo et al., “A survey on deep hashing methods,” ACM Transactions on Knowledge Discovery
from Data, vol. 17, no. 1, pp. 1–50, 2023.
[34]
H. Liu, J. Hagen, M. Ali, and J. Oliver, “An evaluation of malware triage similarity hashes.,” in
ICEIS (1), 2023, pp. 431–435.
[35]
J. Oliver, M. Ali, and J. Hagen, “Hac-t and fast search for similarity in security,” in 2020 Interna-
tional Conference on Omni-layer Intelligent Systems (COINS), IEEE, 2020, pp. 1–7.
[36]
C. Zhao et al., “Deep unsupervised hashing with semantic consistency learning,” in 2023 IEEE
International Conference on Image Processing (ICIP), IEEE, 2023, pp. 1380–1384.
[37]
A. P. Namanya, Q. K. A. Mirza, H. Al-Mohannadi, I. U. Awan, and J. F. P. Disso, “Detection of
malicious portable executables using evidence combinational theory with fuzzy hashing,” in 2016
IEEE 4th International Conference on Future Internet of Things and Cloud (FiCloud), IEEE, 2016,
pp. 91–98.
15