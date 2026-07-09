1
Sharing cyber threat intelligence:
Does it really help?
Beomjin Jin∗, Eunsoo Kim∗, Hyunwoo Lee†, Elisa Bertino‡, Doowon Kim§ and Hyoungshick Kim∗
∗Department of Electrical and Computer Engineering, Sungkyunkwan University
†Department of Energy Engineering, Korea Institute of Energy Technology (KENTECH)
‡Department of Computer Science, Purdue University
§Department of Electrical Engineering and Computer Science, University of Tennessee
∗{jinbumjin, eskim86, hyoung}@skku.edu, †hwlee@kentech.ac.kr, ‡bertino@purdue.edu, §doowon@utk.edu
Abstract—The sharing of Cyber Threat Intelligence (CTI) it into their security operations. The report also unveils that
across organizations is gaining traction, as it can automate nearly 47% of them have dedicated CTI teams. Sharing
threatanalysisandimprovesecurityawareness.However,limited CTI data across organizations is instrumental in countering
empirical studies exist on the prevalent types of cybersecurity sophisticated attacks such as Advanced Persistent Threats
threatdataandtheireffectivenessinmitigatingcyberattacks.We
(APTs). Comprehensive CTI data can be leveraged to identify
proposeaframeworknamedCTI-Lensetocollectandanalyzethe
indicators linked to one or more stages (e.g., reconnaissance,
volume, timeliness, coverage, and quality of Structured Threat
initial compromise, lateral movement, and data exfiltration)
Information eXpression (STIX) data, a de facto standard CTI
of these attacks [47]. For instance, ransomware threats like
format,fromalistofpubliclyavailableCTIsources.Wecollected
about6millionSTIXdataobjectsfromOctober31,2014toApril Locky [24] and WannaCry [44] employ a similar method of
10, 2023 from ten data sources and analyzed their characteris- infection–emails containing malicious attachments or links. A
tics. Our analysis reveals that STIX data sharing has steadily shared characteristic between these attacks is the use of the
increased in recent years, but the volume of STIX data shared Tor network for their command and control communication.
is still relatively low to cover all cyber threats. Additionally, Once a ransomware sample infiltrates a system, it propagates
only a few types of threat data objects have been shared, with laterally,aimingtocompromiseothersinthenetwork.Thekill
malware signatures and URLs accounting for more than 90%
chains for these attacks, which occurred almost a year apart,
of the collected data. While URLs are usually shared promptly,
showed significant overlap; knowledge of the former attack
withabout72%ofURLssharedearlierthanoronthesameday
could have facilitated preparations for the subsequent one.
as VirusTotal, the sharing of malware signatures is significantly
slower.Furthermore,wefoundthat19%oftheThreatactordata Several standards are available to represent and share
containedincorrectinformation,andonly0.09%oftheIndicator threat information in a structured manner, such as Structured
dataprovidedsecurityrulestodetectcyberattacks.Basedonour
ThreatInformationeXpression(STIX)[57],CyberObservable
findings,werecommendpracticalconsiderationsforeffectiveand
eXpression (CybOX) [11], and Malware Information Sharing
scalable STIX data sharing among organizations.
Platforms(MISP)[13].SharingCTIdatainastructuredformat
makesiteasiertoautomatedataprocessingandanalysis.STIX
I. INTRODUCTION
has become the de facto standard [19], [3] for representing
Cyberattackshaveincreasedinnumber,andtheirrisksare CTI data because it was expressly designed to represent
becomingmoresevere[1].Arecentreport[20]revealsa435% various levels of CTI information in a machine- and human-
increase in the number of ransomware attacks from 2019 to readableformat[56].Furthermore,astandardprotocol,Trusted
2020. It is also reported that 68% of industry practitioners felt Automated eXchange of Indicator Information (TAXII) [33],
that cybersecurity risks would threaten their business [2] and facilitates the sharing of STIX data across organizations.
56% of utilities were targeted by cyber attacks [65]. Not only Therefore, we focus on STIX analysis.
isthescaleofcyberattacksgrowing,butthecostsincurredby
Despite the availability of STIX, companies perceive that
victims are also rising. For example, the estimated cost of the
vitalCTI(e.g.,campaigns,motivations,andTTPs)isnoteffec-
Solarwinds attack is about $100 billion [54].
tivelydisseminatedinpractice[25].Arecentsurveyfoundthat
Cyber Threat Intelligence (CTI) has emerged as an indis- many organizations value high-level CTI (e.g., TTPs, threat
pensable component of standard security procedures, assisting actors, security rules, and APT campaigns) more than simple
organizations in risk prioritization, timely detection, mitiga- indicators of compromise (IoCs), such as malware hashes, IP
tion, and containment of attacks [25]. A SANS report [18] addresses,andURLs,fordevelopingeffectivedefensesagainst
indicatesthatnearly60%oforganizationsarealreadyutilizing sophisticated attacks [10]. However, despite the recognized
CTI in their security practices, and 25% plan to integrate importance of high-level CTI, little research has been done to
understand how high-level CTI is shared in the wild. Previous
research in open CTI has predominantly focused on a limited
range of basic IoC data types [26], [27], [17], [73].
NetworkandDistributedSystemSecurity(NDSS)Symposium2024
26February-1March2024,SanDiego,CA,USA
ToaddressthelackofknowledgeontheSTIXdatasharing
ISBN1-891562-93-2
landscape, we collect STIX data from all publicly available
https://dx.doi.org/10.14722/ndss.2024.24228
www.ndss-symposium.org open CTI sources, enabling comprehensive large-scale anal-

2
ysis. This analysis is essential for enhancing the sharing of numberofSTIXdatatypesareused.MostSTIXdatarepresent
advanced cybersecurity threat data, as it will help identify key simple IoCs, such as malware signatures and URLs. This
challenges inhibiting organizations from effectively sharing suggests that the STIX standard is not being used to its full
such data. potential.Wealsofoundthatmanyoftheobjectsandattributes
specified in the STIX standard are not used in practice. For
To this end, we develop a framework named CTI-Lense1
example, only 0.09% of indicators contain security rules that
that collates STIX data from a set of open CTI sources
can be used for detecting cyber attacks in an automated
and systematically analyzes the collected data. The analysis
manner.Thissuggeststhatmoreguidanceisneededonhowto
evaluates the data on four measurements: volume, timeliness,
usetheSTIXstandardtorepresentcomplexthreatinformation.
coverage, and quality on a large scale. CTI-Lense facilitates
more efficient and accurate analysis of the state of open CTI Quality.Weevaluatethequalityofthevaluescontainedinthe
data sharing. To demonstrate the potential of CTI-Lense, we STIX data from two aspects: correctness and completeness.
collectapproximately6millionSTIXdatapointsfromOctober Correctness refers to whether the STIX data are appropriately
31, 2014 to April 10, 2023 (a span of nine years) across 10 used with the right objects, attributes, and values as intended
data sources and conduct a comprehensive analysis of these in the STIX standard. For example, we found that 19% of the
data. We summarize our findings as follows. STIX data have incorrect Threat actor values. Completeness
refers to whether the STIX data accurately contain valid
Volume. STIX data sharing has steadily increased in recent
threat information. For example, we found that over 50% of
years, but the volume of STIX data shared is still relatively
the Indicator objects represent their information in narrative
low to cover all the cyber threats arising. Our investigation
forms as existing threat reports. This suggests that additional
reveals that security service providers generate and share an
manualeffortsarerequiredtoprocessSTIXobjectsandextract
average of only 2,063 unique STIX objects per day. This
relevant threat information.
quantity seems significantly inadequate to handle the daily
influx of new malware samples, malicious IPs, and URLs,
particularly considering that the AV-TEST Institute records II. BACKGROUNDANDMOTIVATION
over 450,000 new malware samples and potentially unwanted
A. What are CTI and STIX?
applicationseachday[6].Furthermore,ourresearchfoundthat
37.89% of STIX objects generated even by a single service Cyber Threat Intelligence (CTI). CTI is evidence-based
provider are duplicated, with JamesBrine exhibiting the knowledge about cybersecurity threats that supports security
highest duplication rate at 68.74%. However, the percentage analystsinmakinginformeddecisions.Itprovidesinformation
of duplicated data across different service providers remains about threats at all levels, from low-level indicators such as
relatively low at 4.10%. Based on these findings, consumers basic IoCs (e.g., malware hashes and IP addresses) to high-
of STIX data should contemplate implementing additional de- level indicators (e.g., TTPs). CTI data is often disseminated
duplication procedures and collecting STIX data from various in unstructured forms, such as narrative reports, blog posts,
sources to enhance the heterogeneity of STIX data. or vendor documents. This requires significant preprocessing,
including deduplication and structurization, before it can be
Timeliness. We find that URL objects are typically dissemi-
used to make any decisions on threats in an automated
nated promptly, with approximately 72% and 88% of URLs
method[10].StructuredCTIs,suchasSTIXandMISP,address
shared either before or on the same day as their appearance
thischallengebyprovidingamachine-readableformatforCTI
on VirusTotal and HybridAnalysis, respectively. This prompt
data.ThismakesitmoreefficienttomanageandanalyzeCTI.
sharingofURLswithinthefirst24hourscouldpotentiallypre-
vent a considerable portion of malicious domains, considering Structured Threat Information eXpression (STIX). The
their typically short lifespans [23]. For example, many spam heterogeneityofCTIspecifications,eachwithitsownstructure
domainsareactiveformerelyaday,aimingtoevadedetection and data expression method, challenges organizations when
and avoid inclusion in blocklists [72]. On the contrary, the sharingintelligence.Toaddressthischallenge,theUSDepart-
sharing of file-type threat objects (e.g., malware signatures) ment of Homeland Security (DHS) and MITRE2 collaborated
is notably slower than that of VirusTotal. This observation todevelopSTIX,astandardthatprovidesastructuredlanguage
indicates potential inefficacy in preventing malware attacks, for expressing threat information.
especiallyconsideringthat54%ofmalwaresamplesareactive
for only 24 hours [48]. In addition, we note that STIX data STIX ecosystem. Figure 1 illustrates the entities within a
is commonly shared two to four days after reporting security STIXecosystem.Astandardizationorganization(e.g.,OASIS)
incidents. Our data show weak evidence of causality [51] publishes the STIX specification (1). A producer (e.g., a
from security incidents reported in sources like Malpedia and security analyst) analyzes cyber threats and generates STIX
security news websites to the sharing of STIX data in our data according to the STIX standard (2). The producer then
collected dataset, with p < 0.05 for the 2–4 and 2–12 days sharestheSTIXdataviaplatforms(e.g.,STIXdatarepositories
time-lag, respectively. However, we do not find any evidence or TAXII servers) managed by a service provider like MITRE
of causality between the publication of CVEs and STIX data. or AlienVault OTX (3). A consumer then fetches the STIX
data from such a platform using a standard protocol such as
Coverage.TheSTIXstandard[57],[9]isdesignedtorepresent Git or TAXII (4). Finally, the consumer employs this data
awiderangeofsophisticatedcybersecuritythreatinformation, to analyze security incidents or minimize the risk of cyber
such as APTs, threat actors, and their relationships. However,
our analysis of shared STIX data shows that only a limited 2MITRE is a non-profit R&D organization supporting various US gov-
ernment agencies in aviation, defense, healthcare, homeland security, and
1https://github.com/SKKU-SecLab/CTI Lense cybersecuritydomains.
2

3
Standard 1 <stix:STIX_Package ...>
organization Producer Service provider Consumer 2 <stix:Indicator
❶ ❷ ❸ ❹ ❺ 3 id="opensource:indicator-..."
4 timestamp="2014-11-05T03:05:02.125545+00:00"
5 version="2.1.1">
CTI Security TAXII server / Company / Attacker 6 <indicator:Title>
standard analyst Web repository Individual 7 ... Reported CnC ...
Fig.1:EcosystemofSTIX.Astandardorganizationisrespon- 8 </indicator:Title>
9 <indicator:Type>IP Watchlist</indicator:Type>
sible for publishing STIX specifications. Producers generate
10 <indicator:Description>
STIX data and upload them to a platform managed by a 11 SNORT Rule ...
dedicated provider. Consumers then access and retrieve this 12 </indicator:Description>
STIX data using an industry-standard protocol such as TAXII. 13 <indicator:Observable idref="...">
14 <cyboxCommon:address_value>
15 78.46.33.91
16 </cyboxCommon:address_value>
17 </indicator:Observable>
threats, for instance, by extracting indicators from the STIX 18 <indicator:Indicated_TTP>
data and implementing them in firewall rules (5). In a real- 19 <ttp:Name> ET.Evil </ttp:Name>
world STIX ecosystem, there are typically multiple service 20 </indicator:Indicated_TTP>
providers,producers,andconsumers.Producersandconsumers 21 <indicator:Test_Mechanisms>
22 <snortTM:Rule>
can be enterprises, financial institutions, or even individuals.
23 alert tcp HOME_NET any ...
24 </snortTM:Rule>
STIX objects and attributes. STIX data comprise various
25 </indicator:Test_Mechanisms>
types of cyber threat information such as observables, indi- 26 <indicator:Producer>
cators, incidents, Tactics, Techniques and Procedures (TTPs), 27 <stixCommon:Name>
exploittargets,coursesofaction,campaigns,andthreatactors. 28 rules.emergingthreats.net
Information within STIX data is depicted as objects and their 29 </stixCommon:Name>
30 </indicator:Producer>
corresponding attributes. Figure 2 shows an example of STIX
31 </stix:Indicator>
1 data representing indicator information, which includes the 32 </stix:STIX_Package>
IPaddressofthecommandandcontrol(C&C)serverrelatedto
ET.Evilmalware,aswellastheSnortrulefordetectingsuch
Fig. 2: Example of a STIX 1 document. The STIX data
networktraffic.Inthisfigure,athreatindicatorisexpressedas
consists of objects (e.g., stix:Indicator) and their attributes
an Indicator object (i.e., <stix:Indicator>. An object’s
(e.g., indicator:Title) written in an XML format (STIX 1) or a
details, such as Title and Type, are represented as attributes
JSON format (STIX 2).
(i.e., <indicator:Title> and <indicator:Type>).
STIX 1 and STIX 2. STIX 1, introduced by MITRE in 2012,
uses an XML format to describe threats and supports nested
(i.e., consumer requesting data to obtain CTI data). A server
structures (i.e., objects within objects). However, these nested
typically manages several data repositories through these two
structures often render STIX data intricate and challenging to
servicestopubliclyshareSTIXdatawithconsumers.However,
parse.Additionally,STIX1allowsformultiplerepresentations
TAXIIdoesnotsupportbackwardcompatibilitywithregardto
ofthesamethreat,leadingtotheexistenceoftwoSTIXobjects
STIX 1 and STIX 2. In other words, a TAXII server cannot
that describe the same target with different structures. To
sharebothversionsofSTIX.AswedealwithbothSTIX1and
overcome the limitations of STIX 1, STIX 2 was standardized
2 data in our analysis, we collect our data from open TAXII
by OASIS in 2017. The main distinction between STIX 1 and
1 and 2 servers, respectively.
STIX 2 is in their structures: STIX 2 uses the JSON format,
making it more lightweight and easier for programmers to
C. Why Do We Focus on STIX?
work with, and it favors a more flat and relational data model
over deeply nested structures. Although the CTI ecosystem Despite the need for advanced threat intelligence (e.g.,
is gradually transitioning to STIX 2, many service providers TTPs, threat actors, security rules, and APT campaigns), there
continue to share STIX 1 data. Hence, it is crucial to analyze is a lack of research on the sharing of structured CTI that
STIX 1 data as well. To perform analysis, we collect and caneffectivelyrepresentsuchinformation.Therefore,wefocus
examine both versions of the STIX data. on STIX in our CTI analysis because it is the de facto
standard for structured CTIs [19], [3] and is widely used in
B. Trusted Automated eXchange of Indicator Information
cybersecurity due to its effectiveness in representing complex
TheTrustedAutomatedExchangeofIntelligenceInforma- threat data [18], [53]. STIX is used in various domains,
tion (TAXII) [33] is a protocol for exchanging CTI data over includingsecuritypolicydevelopment,threathunting,andrisk
HTTPS. TAXII defines APIs, enabling producers (or publish- monitoring. For example, Syam et al. [5] propose a trusted
ers) to share their CTI data with consumers (or subscribers). response management ecosystem where STIX serves as a
TAXIIoutlinestwoprimaryservicestosupportdifferenttypes unifiedlanguageforthreatintelligenceproviderstoseamlessly
ofsharing:(1)aTAXIIcollectionand(2)aTAXIIchannel.A use CTIs for defensive actions. STIX also plays a crucial
TAXII collection is used by CTI producers to store CTI data role in formalizing ontology for CTI, enabling connections
on a data repository managed by a service provider. A TAXII between security events and CTIs, and allowing the inference
channel is employed to exchange CTI data between TAXII of new knowledge about potential threats [40]. Prominent
servers (i.e., publisher pushing CTI data) and TAXII clients entities in the cybersecurity sector, such as SANS [31] and
3

4
Intel[30],incorporateSTIXindicatorsintotheirthreat-hunting External sources CTI-Lense
methodologies. These indicators, such as IP addresses, do- S • T R I e X po r s e it p o o r r y ts XML T X E M X L T Preprocessor V • o D l u u p m li e c a a t n ed a l d y a z t e a r
main names, and host artifacts, serve as the foundation for •TAXII server JSON •Daily/monthly data
formulating security rules for network monitoring. Automated Incidents JSON
alertsgeneratedfromthreat-huntingplatformscanbeexported •CVE TEXT JSON Data Coverage analyzer
via TAXII in a STIX-compatible format, which can then be • • S M e a c l u p r e i d ty i a news manager • • O M b o j n e t c h t/ l a y t t t r r i e b n u d te usage
integrated into security information and event management APT reports co C lle T c I to r Save
(SIEM) systems. •McAfee TEXT Timeliness analyzer
•TrendMicro CTI •vs.security incidents
•Kaspersky database •vs.other services
III. RESEARCHQUESTION&METHODOLOGY
AV reports IoC Query Data
•HybridAnalysis Quality analyzer
This section presents the research questions we aim to •MetaDefender JSON Analyzer •Improper value
answer, the design of our framework (CTI-Lense), and the •VirusTotal •Improper usage
dataset we employ to answer these research questions. Fig. 3: Overview of CTI-Lense.
A. Research Questions
TABLE I: CTI source information used to evaluate STIX data
Our research has two main goals. First, we aim to ex- in CTI-Lense.
amine the volume, timeliness, and coverage of STIX data
disseminated in the public domain to gain insights into its CTI data Format Sources Contained info
usage. Second, we aim to evaluate the quality of STIX data
in accurately representing cybersecurity threat information, STIX XML, • TAXII • STIX 1 obj/attr
including malware, threat actors, and TTPs. To achieve these JSON • Repository • STIX 2 obj/attr
goals, we raise the following research questions: • CVE • Title
Incidents TEXT • Malpedia • Description
RQ1 (Volume): What is the extent of STIX data that
• Security news • Publication time
is being generated and shared publicly? Our objective is
to quantify the amount of STIX data publicly released from • Kaspersky
open sources. Specifically, we examine the count of unique • TendMicro • Malicious IoCs
STIX objects shared daily. This analysis provides insights APT report TEXT • McAfee • Threat actor
into the quantity of shared STIX data to determine if the • ESET • Malware type
volume of publicly shared STIX data is sufficient to cover • Fortinet
a massive number of new cyber threats. Next, we are also • VirusTotal
• AV scan results
interested in how many duplicated data are in the collected AV report JSON • MetaDefender
• First submission time
dataset to see the substantial scale of the shared STIX data. • HybridAnalysis
A lot of redundant data results in a smaller coverage of the
target threats that STIX describes. Furthermore, it means that
securityapplicationsthatrelyonSTIXobjectsshouldperform
RQ4 (Quality): What is the quality of STIX data in
the unnecessary deduplication process. Lastly, we investigate
terms of its correctness and completeness in representing
the number of STIX data over time for multiple sources. It
cyber threats? Our objective is to evaluate the quality of
aimstofindanytrendinSTIXadoption.Wealsofocusonthe
STIX data by assessing its correctness and completeness in
existence of sources where data is consistently shared.
representing crucial cybersecurity information. We investigate
RQ2 (Timeliness): How promptly is STIX data shared if the objects and attributes within the STIX data are used
followingacyberthreatdiscovery?Ourobjectiveistoassess accurately according to the STIX standard, and if producers
the time interval from when a threat is initially detected to assign valid values correctly. This analysis provides insights
when a corresponding STIX object is publicly released since into the reliability of the disseminated STIX data.
the timeliness of threat intelligence plays a pivotal role in
cybersecurity. This analysis provides a clearer perspective on
B. Overview of Our Framework (CTI-Lense)
howefficientlythreatintelligencesharingcanactasadeterrent
against cyber attacks. To answer our research questions, we developed CTI-
Lense, a framework that aggregates CTI data from multiple
RQ3 (Coverage): To what extent does publicly shared
open sources and analyzes the compiled data to assess its vol-
STIX data cover a wide range of cybersecurity threats?
ume, timeliness, coverage, and quality. We use CTI-Lense to
Our objective is to evaluate the coverage of threat information
analyze STIX data. Figure 3 illustrates the overall architecture
disseminated via the STIX standard. STIX defines a wide
ofCTI-Lense,whichcollectsCTIdata(particularlySTIXdata)
range of objects and attributes to represent different aspects
fromvarioussourcestobetterunderstandtheSTIXecosystem.
of cybersecurity threats, from basic IoCs (e.g., malicious IPs
The format, full list of sources, and information for each CTI
and file hashes) to sophisticated TTPs (e.g., threat actors and
data are listed in Table I.
malware categories). We analyze the distribution of STIX
objects and attributes in a large dataset of publicly available We primarily analyze STIX as our CTI dataset, evaluating
STIX data. This analysis provides a deeper understanding volume, timeliness, coverage, and quality based on its objects
of the extent to which the STIX standard is being used to andattributes.The“incident”datasetisanarrativetextdataset
represent a wide range of cybersecurity threats. sourced from Malpedia, security news websites, and CVEs.
4

5
It contains the title, description, and publication time, which
TABLE II: List of STIX data sources (TAXII servers (T)
we use to perform causality tests to measure timeliness.
and repositories (R)), the number of total and unique objects
The “APT report” dataset is another narrative text dataset
collected from each source, and the proportion of duplicated
reported by security experts from prominent companies, such
(dup.)objectsineachSTIXdatasourcewithinthesamesource
as Kaspersky. This dataset provides information on malicious
(In dup.) and across different sources (Out dup.).
IoCs, threat actors, and malware types and helps to evaluate
the quality of the STIX data. For text-based CTI datasets
STIXsources Total Unique Indup. Outdup.
like “incident” and “APT reports,” we transform them into
JSONformat,incorporatingbothrawtextandessentialdetails STIX1
HailaTAXII(T) 3,820,542 1,900,237 50.71% 0.76%
for comparison with STIX data. Finally, the “AV report”
AlienVaultOTX(T) 2,697,680 1,647,509 38.93% 1.16%
dataset is a JSON-formatted CTI dataset collected from three IBMX-ForceExchange(T) 628,738 273,274 46.36% 20.32%
widely used scanning services: VirusTotal, MetaDefender, and PickupSTIX(T) 100,859 73,575 18.13% 6.86%
HybridAnalysis. It contains anti-malware scanning results and STIX2
IoC submission times, which we use to analyze the timeliness AlienVaultOTX(T) 1,739,017 1,657,442 3.02% 2.00%
and quality of the STIX data. JamesBrine(R) 658,282 205,776 68.74% 0.00%
DigitalSide(R) 298,598 198,439 33.52% 7.78%
Volume. We assess the volume and duplication of STIX data Cyware(T) 263,633 228,782 11.94% 2.51%
IBMX-ForceExchange(T) 122,717 119,611 1.25% 2.54%
tounderstandtheamountandqualityofinformation.First,we
Unit42(T) 38,201 33,379 7.03% 13.29%
count the number of STIX data objects shared daily and by MITREATT&CK(R) 16,936 17,042 0.05% 1.73%
source. Then, we measure the proportion of duplicated STIX LimofromAnomali(T) 7,170 6,492 0.06% 12.68%
data within and across sources by comparing objects based PickupSTIX(T) 516 507 1.74% 0.00%
on unique attribute values. Finally, we measure the number of
monthly shared STIX data for each source (see Section IV).
entities by comparing each indicator object’s value with the
Timeliness.WeassessthetimelinessofSTIXdatabycompar- scanning reports obtained from online scanning services. We
ingitspublicreleasetimewiththetimingofsecurityincidents use three prominent online scanning services, VirusTotal [71],
and its registration time to popular scanning services such as HybridAnalysis [12], and MetaDefender [46], to determine
VirusTotal[71].ThisallowsustodeterminehowquicklySTIX whether a given data is malicious using a threshold-based
data is shared compared with commercial services. First, we method[64].Improperusagereferstocaseswherevaluesexist
investigate how long it takes for STIX data to be publicly re- inanobjectbutarenotassignedtothepreciseattributes,which
leased after a security incident occurs. We collect information violates the completeness of STIX. For example, a malware
about security incidents from three sources: Malpedia [16], family name is described in the description attribute in
security news websites [35], [29], and CVEs [7]. We also a Report object rather than the name attribute in a Malware
performaGrangercausalityanalysis[62]todeterminewhether object. We evaluate improper usage by investigating whether
the occurrence of security incidents causes more sharing of valuesareincorrectlyassignedtoimproperobjectsorattributes
STIXdataobjects.Second,wecomparethepublicreleasetime when they exist (see Section VII).
of STIX data with its registration time to popular scanning
services such as VirusTotal [71], HybridAnalysis [12], and C. Dataset
MetaDefender [46] (see Section V).
To conduct our analysis, we compiled a dataset of STIX
Coverage.WeassessthethreattypecoverageinSTIXdataby data from seven publicly available TAXII servers and three
measuring the number of objects and attributes producers use repositories, as outlined in Table II. The observation period
to represent cybersecurity information. We count the objects spanned from October 31, 2014 to April 10, 2023, during
andattributesinourcollecteddatasetandcomparethemtothe which we gathered a total of 6,362,065 objects (3,894,595
total specified in the STIX standard. We analyze object and from STIX 1 and 2,467,470 from STIX 2). At first glance,
attribute usage statistics to see which types are used, and how the quantity of STIX data instances we gathered may appear
thesehavechangedovertime.Thisallowsustounderstandthe tobesmallerthanthenumberofreportedIoCsintheprevious
threat information types used in STIX data and if the STIX study [70]. However, a direct comparison between these two
standard is utilized to its full potential (see Section VI). statistics is not straightforward. The IoCs reported in [70]
contain all IoCs provided by producers, a significant portion
Quality. We assess the quality of STIX data by examining
of which would be duplicates. In contrast, our study focuses
improper values and improper usage. Improper values refer
exclusively on refined and actively shared CTI data provided
to instances of incorrect values that violate the correctness of
by service providers, as illustrated in Figure 1.
STIX. We evaluate improper values by investigating whether
the information associated with objects or attributes is correct We selected seven TAXII servers: AlienVault OTX,
and accurate. For keyword type values (e.g., threat actors and Hail a TAXII, IBM X-Force Exchange, Cyware,
malware categories), we obtain valid keywords as correct in- PickupSTIX, Unit42, and Limo from Anomali;
formation from diverse trustworthy data sources. We consider along with three public repositories: JamesBrine,
several valid keywords because several producers use their DigitalSide, and MitreAttack. To aggregate as
ownuniqueattributevalues. Forexample,someproducersuse many public STIX data sources as possible, we refer to
“Lazarus Group,” while others use “Guardians of Peace” or the services listed on the official STIX website [43] and
“Whois Team.” We check whether a given keyword matches data sources referenced in academic literature [3] and public
one of these valid keywords. We also check whether indicator communities [66], [42]. There are 84 STIX-related services
objects (e.g., malware hash, IP, and URL) refer to malicious in total, and we finally select 10 services among them.
5

6
The reasons why we remove 74 services are because 23 of For STIX 1 data, Hail a TAXII emerged as the domi-
them are inaccessible and 19 of them are confined to closed nant source, contributing 3,820,542 objects, which represent
communities or necessitate payment. The rest 32 services are 52.70% of the total 7,249,819 objects. The second largest
ones that only leverage STIX (e.g., SIEM) but do not share contributorwasAlienVault OTX,withitsshareamounting
their STIX data through a TAXII server or a repository. to 2,697,680 objects or 37.21%. IBM X-Force Exchange
andPickupSTIXtogetheraccountedforapproximately10%
TofacilitateeffectiveanalysisofSTIXobjects,westorethe
of the shared data, with contributions of 8.67% and 1.39%,
contentofthecollectedSTIXobjectsinourinternalMongoDB
respectively. For STIX 2 data, 2,171,254 objects (69.04%)
database. To handle two major STIX versions – STIX 1.x and
were shared via TAXII, and external repositories contributed
STIX2.x,whichdifferinstructure(flatvs.nested)andformat
973,816objects(30.96%).AmongtheentitiesthatshareSTIX
(XML vs. JSON), we convert STIX objects that are formatted
2dataoverTAXII,AlienVault OTXwasthemostprolific,
in XML (i.e., STIX 1) into JSON-encoded ones using the
contributing1,739,017objects,whichrepresentmorethan80%
python-stix3 library.
of the shared data.
STIX allows for references within objects. However, these
ForallSTIXobjectsinourdataset,weobservedthatmore
references can have varying styles, complicating processing.
than 9,419,073 objects (90.63%) were shared through TAXII
Forinstance,aSTIX1objectfromAlienVaultincorporates
and973,816objects(9.37%)weresharedfromexternalrepos-
an Observable object as one of the attributes within an Indi-
itories. The top 3 most frequent data sources over TAXII are
cator object. Conversely, Hail a TAXII generates separate AlienVault OTX, Hail a TAXII, and IBM X-Force
Observable objects and references them within an Indicator Exchange,sharing4,436,697(47.10%),3,820,542(40.56%),
object using the id attribute from the Observable objects. We
and 751,455 (7.98%) of the total shared STIX objects, re-
tackle this challenge by ensuring all Observable objects exist spectively. For external repositories, JamesBrine was the
as independent entities with unique ids when we store STIX
most prevalent, sharing 658,282 objects (67.60%), followed
objects in our database. by DigitalSide, sharing 298,598 objects (30.66%), and
Thefinalstepinourdatacollectionprocessisdeduplicating MITRE ATT&CK, sharing 16,936 objects (1.74%).
theSTIXobjects.Thisisanotherchallengingtaskbecausedif- Data duplication.TableIIshowsthenumberofSTIXobjects
ferentcollectionscanhavedifferentSTIXobjectsrepresenting sharedbyeachdatasourcefromthemomentwefirstobserved
thesamethreatinformation,evenwithdifferentidentifiers.For their sharing activity to the end of our data collection period.
example, two STIX objects might both represent a malware
sample, but they might have different identifiers because Our analysis of unique objects shared by data sources in
they were collected from different sources. To address this both the STIX 1 and STIX 2 data reveals interesting trends.
challenge, we follow a two-step deduplication process. First, Contrary to what one might expect, the volume of shared ob-
weremoveobjectswithidenticalidentifiersthatwerecollected jects does not directly correlate with the number of duplicates
from the same data source. This ensures that we do not have across various data sources. For instance, Hail a TAXII,
multiplecopiesofthesameobjectfromthesamesource.Sec- which shares the highest number of objects, has the lowest
ond, we eliminate duplicate objects based on unique attribute proportion of duplicates at only 0.76%. This is followed by
values. For example, we might compare the hash values of AlienVault OTX at 1.16%, PickupSTIX at 6.86%, and
malware samples or the IP addresses of malicious domains.
IBM X-Force Exchangeat20.32%.Similarly,inSTIX2,
This ensures that we do not have multiple copies of the same each data source possesses a significant number of uniquely
object from different sources. After deduplication, our dataset shared STIX objects not found in other data sources. These
comprises 6,362,065 objects, which were deduplicated from results are consistent with previous work showing that most
10,392,889 objects. data in threat intelligence feeds are unique [41], [68], [26].
However, we observe a significant amount of duplication
IV. VOLUMEOFSTIXDATAINTHEWILD withinindividualdatasources.Forinstance,Hail a TAXII
shows a 50.71% duplication rate in its shared STIX 1 ob-
This section provides several statistical measures of pub- jects. Other sources present similar trends: IBM X-Force
licly shared STIX data using CTI-Lense. We first quantify the Exchange contains 46.36% duplicates, AlienVault OTX
volume of STIX data by measuring the number of publicly contains 38.93%, and PickupSTIX contains 18.13%. Con-
shared STIX objects, analyzing in more detail with regard to sidering that Hail a TAXII and AlienVault OTX to-
STIX version and data source. Second, we analyze the extent
gether account for 89.91% of all shared STIX 1 objects, the
of duplicated data within the dataset. Finally, we measure
high rate of duplication within these sources raises concerns
the number of monthly shared STIX data for each source to
about data redundancy.
analyze the volume trend over time.
Incontrast,therateofduplicatesinSTIX2datasharedvia
Number of STIX objects shared. In our dataset, we find TAXII is relatively lower, but the concentration of duplicates
that the first shared STIX data appeared on October 31, 2014. ishigherfordatasharedthroughexternalrepositories.Among
In total, 10,392,889 STIX objects were shared until April these, JamesBrine shared the highest number of STIX
10, 2023, equating to an average daily share of 3,371 STIX 2 objects (658,282), with 68.74% of which are duplicates.
objects.OfthetotalnumberofsharedSTIXobjects,7,247,819 DigitalSide, the second most prevalent repository, shared
(69.74%)areinSTIX1formatand3,145,070(30.26%)arein 298,598 objects, 33.52% of which are duplicates.
STIX 2 format.
Overall,37.89% ofthe STIXobjects generatedby asingle
3https://github.com/STIXProject/python-stix sourceareredundant,withAlienVault OTXexhibitingthe
6

7
106
105
104
103
102
101
100
Jun
201
D
5
ec
201
J
5
un
201
D
6
ec
201
J
6
un
201
D
7
ec
201
J
7
un
201
D
8
ec
201
J
8
un
201
D
9
ec
201
J
9
un
202
D
0
ec
202
J
0
un
202
D
1
ec
202
J
1
un
202
D
2
ec
2022
atad
XITS
#
AlienVault OTX Hail a TAXII IBM X−Force Exchange Total
(a) STIX 1.
106
105
104
103
102
101
100
Jun
201
D
5
ec
201
J
5
un
201
D
6
ec
201
J
6
un
201
D
7
ec
201
J
7
un
201
D
8
ec
201
J
8
un
201
D
9
ec
201
J
9
un
202
D
0
ec
202
J
0
un
202
D
1
ec
202
J
1
un
202
D
2
ec
2022
atad
XITS
#
STIX 2 data that shared identical hash values, confirming that
STIX 1 data was utilized in the generation of STIX 2 data.
Overall, the trend of sharing STIX data has been rising since
2016.
We apply linear regression to our dataset to see the trend
in the number of shared data for each source over time. We
foundthatthevolumeofshareddataforAlienVault OTX,
IBM X-Force Exchange, JamesBrine, and Cyware
graduallyincreasedwithp<0.05.Onthecontrary,thevolume
of shared data for Hail a TAXII suddenly decreased with
p<0.05. However, this is because Hail a TAXII stopped
sharingSTIXdatainJune2022.Furthermore,weobservethat
AlienVault OTX DigitalSide JamesBrine
Cyware IBM X−Force Exchange Total the total amount of shared data in STIX 2 gradually increases
with p<0.05, while the trend in the total amount in STIX 1
is not statistically significant.
Takeaway: STIX 1 is shared more often than STIX
2, but the sharing of STIX 2 data has been signifi-
cantly increasing recently, along with new sources. The
data is primarily sourced from leading security compa-
nies like AlienVault OTX, Hail a TAXII, and IBM
X-Force Exchange, mainly through TAXII. This con-
(b) STIX 2. centrationimpliesthattheopenSTIXdatasetpredominantly
depends on a few sources. This concentration implies that
Fig. 4: Monthly number of shared STIX data. The red zones theopenSTIXdatasetlargelyreliesonafewsources.While
indicate periods of rapid increase in STIX data volume, while there is a low level of duplicate data across providers, sub-
the blue zone indicates a period of rapid decrease in volume. stantial duplication is observed within individual providers.
This emphasizes the need to remove duplicate data before
deployment. Overall, we observe that the trend of sharing
highest duplication rate at 68.74%. Of the total 10,392,889
STIX data has been rising since 2016.
shared STIX objects, only 6,362,056 (61.22%) are unique
across all data sources. Consequently, a daily average of
just 2,063 unique objects is publicly shared. Given the daily
V. TIMELINESS
influx of new malware samples, IPs, and URLs, this number
seems insufficient, especially considering AV-TEST Institute’s This section investigates the timeliness of STIX data,
recordsindicatingthatover450,000newmalwaresamplesand utilizingCTI-Lense.First,weanalyzetherelationshipbetween
potentially unwanted applications emerge each day [6]. the STIX data and corresponding security incidents. Then, we
analyze the latency between the initial appearance of STIX
STIX data sharing volume over time. Figure 4 shows how dataanditssubsequentdetectionbypopularscanningservices.
many STIX data (1 and 2) have been shared by each source
duringourobservationperiod,excludingsourceswithlessthan
A. STIX and Security Incidents
100,000 unique data for better presentation.
We examine the relevance of STIX data to actual security
STIXdatasharinghasincreasedinrecentyears.Asshown
incidents. Timely threat information is crucial for consumers.
in Figure 4a, over 10,000 STIX data points have been shared
Therefore, we first explore the latency between the date that
on Hail a TAXII in most months since January 2015.
security incidents are initially detected and the date when
AlienVault OTX experienced a sharp rise in shared STIX
the corresponding STIX object is generated. Subsequently, we
1 data starting in June 2016, with nearly 10,000 STIX data
analyzetheGrangercausality[62]betweenthedailynumberof
pointsbeingsharedinmostmonthsafterthat.IBM X-Force
sharedSTIXobjectsandthedailynumberofsecurityincidents
Exchange has consistently shared over 1,000 STIX data
reported from three distinct sources: Malpedia [16], security
pointsmonthlysinceMarch2018.Thesethreesourcessignifi-
news websites, and the publication of CVEs. We perform this
cantly contributed to the dataset and consistently shared STIX
analysis at various time lags. Based on the time each security
data until Hail a TAXII stopped sharing in June 2022. As
incident is detected, we measure the number of days until the
shown in Figure 4b, for STIX 2, Cyware, DigitalSide,
corresponding information is first shared in the STIX data for
IBM X-Force Exchange, and JamesBrine have ac-
each data source.
tively shared data since the release of the latest STIX 2.1
version in March 2020 [9]. Because STIX 2 was released in Security incidents. To measure the timeliness of STIX data
July 2017, there should technically be no data created before sharing against security incidents, we focus on two infamous
thisdate.WehavefoundcaseswherethecreationtimeofSTIX cyber attacks: the Mirai botnet [4] and the WannaCry ran-
2 data is earlier than July 2017. This occurred because some somware [61]. Specifically, we measure the time (i.e., the
STIX1datawasconvertedtoSTIX2afterJuly2017,resulting numberofdays)ittookforthehashvalueofamalwaresample
intheretentionoftheoriginalcreationtimeinthenewformat. for each attack to be shared as an attribute value in the STIX
WeverifiedthisbyextractingindicatorsfrombothSTIX1and object, starting from the date of detection. We then compared
7

8
1000
TABLE III: Number of days it takes for STIX data to be
800
produced after a security incident occurs. 600
400
Mirai botnet WannaCry
STIX sources 200
Initial Variant Initial Variant
0
OTXAlienVault 55 10 96 30 −200
IBMX-ForceExchange 43 30 109 50 −400
the STIX data creation date delays of initial malware samples
HA−Domain HA−File HA−IP HA−URI MD−File VT−File VT−URI
with their variants.
Toidentifyinitialmalwaresamplehashvalues,wereferred
tothesecurityreportsofrenownedcompaniesforMiraibotnet
(6b7b6ee71c8338c030997d902a2fa593) and Wan-
naCry (9c7c7149387a1c79679a87dd1ba755bc) [39],
[60]. To calculate the time delay, we searched the malware
hash value on VirusTotal and determined its first detected
time using first submission time. For malware variants, we
randomly selected 100 malware hashes labeled Mirai and
WannaCry in STIX data for each source and measured the
average time difference with VirusTotal in the same way.
Table III summarizes our findings on the time delays.
Only two sources, AlienVault OTX and IBM X-Force
Exchange, contained initial malware sample hash informa-
tion for Mirai and WannaCry. IBM X-Force Exchange
produced the initial Mirai malware hash 43 days after the
initial detection, which is 12 days faster than AlienVault
OTX. In contrast, AlienVault OTX produced the initial
WannaCry sample hash 96 days after the initial detection,
which is 13 days faster than IBM X-Force Exchange.
Unlike initial malware samples, we found that STIX data for
thevariantswassharedmorequicklyforboththeMiraibotnet
and WannaCry. AlienVault OTX shared STIX data for
Mirai botnet and WannaCry variants within an average of 10
and 30 days, respectively, while IBM X-Force Exchange
shared it within 30 and 50 days, respectively.
Causality test. Analyzing the relationship between STIX ob-
jects and security incidents can be challenging due to missing
attributesorunstructuredtextintheshareddata.Alternatively,
we collect daily security incidents from various sources and
conduct causality tests with daily shared STIX data.
To obtain the daily number of security incidents, we
collect12,133postsfromMalpedia,15,671postsfrompopular
news websites, and 151,431 published CVEs within the same
period as the STIX dataset. Malpedia [16] is a resource for
malwareinformation,providingdetailssuchasmalwarefamily
and threat actor information. Furthermore, it promptly shares
information about security incidents from various sources,
including well-known cybersecurity companies (e.g., Avast,
Bitdefender),newswebsites(e.g.,Threatpost),andblogs.This
information includes reports of cyber attack campaigns such
as targeted attacks and critical vulnerabilities.
We utilize Granger causality [62], a statistical concept
of causality used to identify if one time series can predict
another, to measure the relationship between STIX data and
security incidents. We define a time-lag as the number of
days between the date a security incident occurs and the date
when the exchange rate of STIX data increases. Then, we
)syad(
ssenilemiT
Fig. 5: Differences in the data generation time between STIX
and each scanning service. VirusTotal (VT), HybridAnalysis
(HA), and MetaDefender (MD).
analyze causality by varying the time lag from one day to
30 days. To address the multiple comparison problems, we
apply Bonferroni corrections [45], with n = 90, since we
analyze causality with a time-lag up to 30 days for three
incident data sources. We find weak evidence of a causal
relationship between security incidents reported on Malpedia
and security news websites and the sharing of STIX data in
our collected dataset. Specifically, we find that STIX data
are shared after 2–4 days and 2–12 days, with p < 0.05,
following the reporting of incidents in Malpedia and security
news websites, respectively. However, we do not find any
significant evidence suggesting a causal relationship between
thepublicationofCVEsandSTIXdatasharing,withp>0.05
for all time-lag values.
B. STIX and Other Services
WealsocomparethegenerationtimeofSTIXdatawiththe
submission time of corresponding data on popular scanning
services, such as VirusTotal [71], HybridAnalysis [12], and
MetaDefender [46]. These platforms serve as widely used
repositories for virus samples. We concentrate specifically on
the Observable attributes present in Indicator objects, as the
Observable attributes contain one of four types of data: an IP
address, a domain, a file hash, or a URL.
In total, we collect 70,997 file hashes and 374,973 URLs
from VirusTotal, 31,290 file hashes from MetaDefender, and
14,924 domains, 10,179 file hashes, 836 IPs, and 8,055 URLs
from HybridAnalysis. Then, we compare the timestamp at-
tributesoftheIndicator objectswiththefirst submission time
values of VirusTotal and scan start time values in Hybrid-
AnalysisandMetaDefender.WeexcludetheObservabletypes
(e.g.,thoseinIPanddomainobjectsforVirusTotal)forwhich
the first submission time and scan start time are not shown
in the results for each service.
Figure 5 shows the time differences between STIX data
generation and scanning service data submission for each data
type.Forexample,HA-Domainrepresentsthenumberofdays
between when a domain data appeared in HybridAnalysis and
thesamedomaindataappearedinSTIX.Thesedifferencesare
calculated by subtracting the STIX data generation time from
the data submission time for each service. A concentration of
positive values for timeliness indicates that STIX data gener-
ation is quicker than the submission time of the services (e.g.,
HA-Domain). As shown in Figure 5, STIX data for domains,
8

9
filehashes,andURIsisgeneratedequallyormorequicklythan
TABLE IV: Objects and attributes used in the STIX dataset.
HybridAnalysis,whileIPdataisgeneratedmoreslowly.STIX
We find that only 6 (75%) and 15 (75%) types of objects are
alsoshowscomparableperformancetoVirusTotalforURIdata
used in the STIX 1 and STIX 2 datasets, which means that
onaverage,sharing72%ofURLseitherbeforeoronthesame
the full range of STIX objects is not being utilized.
day. However, STIX is much slower than MetaDefender and
VirusTotal with respect to the first submission date for file
STIX version Objects Attributes
hash data. This is a critical issue, as antivirus response times
can be slow, as reported in a previous study [8] that found STIX 1 Count Prop. Usage Prop.
an average antivirus response time of 19 days. These results
Course of action 3,768 0.10% 5 / 19 26.32%
suggest that utilizing STIX data could enhance the efficiency
Exploit target 10,789 0.28% 6 / 15 40.00%
of prompt detection and defense against attacks involving
Incident 29,086 0.75% 4 / 34 11.76%
domains and URIs. However, the efficacy of IP and file hash Indicator 3,846,499 98.77% 13 / 25 52.00%
data is questionable. Threat actor 719 0.02% 3 / 20 15.00%
TTP 3,734 0.10% 10 / 18 55.56%
Takeaway:OuranalysisshowsthatproducersofSTIXdata
actively generate and share them within 2–4 days after a STIX 2 Count Prop. Usage Prop.
security incident occurs. However, except for domain and
Attack pattern 1,662 0.07% 12 / 18 66.67%
URIdata,theefficacyofSTIXisnotyetconfirmed,asmost Campaign 151 0.01% 9 / 20 45.00%
STIX data are generated considerably slower than other Course of action 1,181 0.05% 11 / 16 68.75%
commercial services. Identity 1,110 0.04% 12 / 20 60.00%
Indicator 2,342,261 94.93% 18 / 23 78.26%
Intrusion set 211 0.01% 12 / 23 52.17%
Location 1 0.11% 9 / 25 36.00%
VI. COVERAGE
Malware 2,733 0.21% 16 / 26 61.54%
Observed data 5,087 0.00% 11 / 19 57.89%
This section presents the evaluation of the application of
Report 57,165 2.32% 14 / 19 73.68%
the STIX standard in terms of coverage, utilizing CTI-Lense.
Threat actor 723 0.03% 10 / 27 37.04%
AlthoughSTIX1andSTIX2define8and20typesofobjects, Tool 491 0.02% 11 / 20 55.00%
respectively,only6(75%)and15(75%)objecttypesareused Vulnerability 5,755 0.23% 11 / 16 68.75%
intheirrespectivedatasets.Thisindicatesthatthefullrangeof Sighting 1,273 0.05% 12 / 22 54.55%
STIX objects is not being utilized. Examples of unused object Relationship 47,666 1.93% 11 / 20 55.00%
types include Campaign and Report in STIX 1, and Malware
Analysis in STIX 2.
contain suspicious IP addresses and texts. Type and Indi-
Wealsoanalyzethedatasettoidentifythetypesofobjects cated TTP are used to describe the types of information (e.g.,
most frequently used by those producers (see Table IV). malware family) with several pre-defined keywords, such as
Basedonourquantitativeanalysis,weobtainedthreeimportant IP Watchlist(i.e.,suspiciousIPaddresses)andET.Evil
findings. First, we find that the most widely used object type (i.e., a name of a malware family). Test Mechanism presents
is Indicator, which accounts for more than 90%, regardless rules for detecting Observable.
of the versions of the STIX dataset. The reason why objects
of Indicator type are so widely used is that they contain TableVlistsattributeswithdataontheirusageinIndicator.
observables, such as hash values of malware or URL strings, Almost all the STIX 1 Indicator objects contain the Title
which are used to identify specific malicious samples (see (100%) and Observable (99.92%) attributes. However, not
Figure2).Second,wefindsomecasesinwhichthenumberof manyobjectsincludeinformationtypes,suchasType(53.73%)
similardataincreasesfast.Forexample,aCerber ransomware orIndicated TTP(34.76%).Furthermore,only0.09%ofIndi-
variant was generated every 15 seconds [15] as the URLs cator objectscontainsomerulesintheTest Mechanisms(e.g.,
used are frequently changed by domain generation algorithms Snort rule) attribute for detecting suspicious data. Similarly,
(DGA). Finally, most STIX objects contain less than 50% of most of the STIX 2 data includes the name (100%) and
the attributes defined in STIX 1, while most STIX 2 objects pattern (100%) attributes, which correspond to the Title and
usemorethan50%oftheattributes.Althoughitseemsthatthe Observable attributes in STIX 1. By contrast, labels (32.62%)
number of attributes used in STIX 2 is higher compared with and indicator types labels (17.13%) were seldom used or not
thatofSTIX1,wefindthatthemainreasonforsuchdifference used at all.
is that STIX 2 defines several additional but simple metadata,
STIXobject/attributecoverageovertime.Weanalyzeobject
such as Created, Modified, and Spec version as shown in
and attribute usage for each source to analyze the coverage
Table V. We observe that an average of 2 and 6 attributes
trendovertime.STIXobjecttypeusagepatternsacrossvarious
for STIX 1 and STIX 2 contain metadata, respectively. Other
sources show a degree of consistency, with occasional devia-
than that, the numbers of attributes used in STIX 1 and STIX
tions. Figure 6 shows the monthly average number of objects
2 are similar, which means that producers still do not benefit
used from each source from January 2015 to December 2022.
from newly added objects and attributes.
For better visualization, we have excluded data from STIX
Attributes usage in Indicator. As the Indicator type is the sources with fewer than 100,000 unique objects. We observe
most used in the STIX dataset, we take a further look into the that the number of object types and attributes provided by
attributesusedinIndicator.Figure2includesanexampleofan each source also increases overall, but there remains a limited
Indicator object in the STIX 1 dataset. Title and Description number of object types over time.
9

10
5
TABLE V: Attributes used in Indicator objects. We note that
producers use only certain attributes to describe indicators, 4
andfewSTIXdataincludesecurityrulesorinformationabout
attack steps useful to detect cyber attacks. 3
2
STIX version Attributes Count Prop.
1
Id 3,846,499 100.00%
Title 3,846,499 100.00% 0
O
Ti
b
m
se
e
r
s
v
ta
a
m
bl
p
e 3
3
,
,
8
7
4
7
3
3
,
,
3
0
9
8
1
0
9
9
9
8
.
.
9
0
2
9
%
% Jun
201
D
5
ec
2015
Jun
201
D
6
ec
201
J
6
un
201
D
7
ec
201
J
7
un
201
D
8
ec
201
J
8
un
201
D
9
ec
2019
Jun
202
D
0
ec
202
J
0
un
202
D
1
ec
202
J
1
un
202
D
2
ec
2022
Description 2,346,868 61.01%
Type 2,066,834 53.73%
STIX1 Producer 1,972,657 51.28%
Version 1,899,238 49.38%
Indicated TTP 1,337,050 34.76%
Confidence 1,308,416 34.02%
Short description 1,140,838 29.66%
Test Mechanism 3,482 0.09%
Sightings 116 0.00%
Id 2,342,261 100.00%
Created 2,342,261 100.00%
Modified 2,342,261 100.00%
Pattern 2,342,261 100.00%
Type 2,342,261 100.00%
Valid from 2,342,261 100.00%
Name 2,342,217 100.00%
Description 2,170,475 92.67%
Pattern type 2,005,272 85.61%
STIX2
Spec version 2,001,972 85.47%
Pattern version 1,999,546 85.37%
Labels 763,935 32.62%
Indicator types 401,322 17.13%
Kill chain phases 401,286 17.13%
Created by ref 238,844 10.20%
Object marking refs 98,700 4.21%
Revoked 62,759 2.68%
Valid until 33,503 1.43%
For STIX 1, AlienVault OTX consistently used either
Indicator and Threat actor or Indicator and Incident. IBM
X-Force Exchange used up to three object types by De-
cember 2020, but since June 2021, it has primarily used only
the Indicator type. Hail a TAXII used only the Indicator
type since 2015 and until June 2022, when it appears to have
ceased its sharing service.
ForSTIX2,AlienVault OTXmostlysharestwoobject
types, with a slight increase in the average number recently.
IBM X-Force Exchange consistently shares just one ob-
ject type, while Hail a TAXII does not share any STIX 2
data. Cyware, JamesBrine, and DigitalSide showed
a sudden increase in the number of objects used, indicating
service initiation at that point. After June 2020, Cyware
observed an increase in the average number of shared objects
from 2–3 to about 4.
Our linear regression analysis indicated a statistically sig-
nificant increase in the average number of STIX object types
used across all sources, except for Hail a TAXII in STIX
1 and JamesBrine in STIX 2, with p < 0.05. Hail a
TAXII showed a declining trend, likely due to the cessation
of their service after June 2022.
htnom
rep
sepyt
tcejbo
#
gvA
AlienVault OTX Hail a TAXII IBM X−Force
(a) STIX 1.
5
4
3
2
1
0
Jun
201
D
5
ec
2015
Jun
201
D
6
ec
201
J
6
un
201
D
7
ec
201
J
7
un
201
D
8
ec
201
J
8
un
201
D
9
ec
2019
Jun
202
D
0
ec
202
J
0
un
202
D
1
ec
202
J
1
un
202
D
2
ec
2022
htnom
rep
sepyt
tcejbo
#
gvA
AlienVault OTX Cyware DigitalSide IBM X−Force JamesBrine
(b) STIX 2.
Fig. 6: Average number of STIX object types used for each
source per month. The red zones indicate periods of rapid
increase in STIX object types used, while the blue zone
indicatesaperiodofrapiddecreaseinSTIXobjecttypesused.
STIX attribute usage patterns in Indicator objects show
similar trends. Figure 7 shows the average number of at-
tributes used in Indicator objects for each source per month.
We performed linear regression on STIX attribute usage in
Indicatorobjectspersourceandfoundastatisticallysignificant
increase in the average number of STIX attributes used across
allsources(p<0.05),exceptforAilenVault OTXinboth
STIX versions and IBM X-Force Exchange in STIX 2.
AlienVault OTX showed no significant trend, and IBM
X-Force Exchangegraduallydecreaseditsattributeusage
(p<0.05).
Interestingly, different sources use different object types
andattributestorepresenteventhesameCTIdata,likelydueto
variationsinSTIXgeneration.Eachsourcehasitsownunique
web interface for collecting CTI information, with varying
options and defaults. These differences appear to have led to
unique object types and attribute patterns for each source.
Takeaway: Our analysis shows that the STIX data are
mostly based on the Indicator objects, accounting for more
than 90% of all the data. Most of the Indicator objects
contain simple indicators of compromise information, such
as malicious file hash or URL strings, while few of them
include security rules to detect cyber attacks or information
about attack steps to detect or predict multistep attacks.
Also, we find that the number of object types and attributes
usedforSTIXdataisdiversedependingonthedatasource.
10

11
15
12
9
6
3
0
Jun
201
D
5
ec
201
J
5
un
201
D
6
ec
201
J
6
un
201
D
7
ec
201
J
7
un
201
D
8
ec
201
J
8
un
201
D
9
ec
201
J
9
un
202
D
0
ec
202
J
0
un
202
D
1
ec
202
J
1
un
202
D
2
ec
2022
htnom
rep
desu
srtta
#
gvA
AlienVault OTX Hail a TAXII IBM X−Force
(a) STIX 1.
15
12
9
6
3
0
Jun
201
D
5
ec
201
J
5
un
201
D
6
ec
201
J
6
un
201
D
7
ec
201
J
7
un
201
D
8
ec
201
J
8
un
201
D
9
ec
201
J
9
un
202
D
0
ec
202
J
0
un
202
D
1
ec
202
J
1
un
202
D
2
ec
2022
htnom
rep
desu
rtta
#
gvA
1 <stix:Indicator id="indicator-...">
2 <indicator:Observable idref="...">
3 <cyboxCommon:simple_hash_value>
4 12cc14bbbc421275c3c6145bfa186dff
5 </cyboxCommon:simple_hash_value>
6 </indicator:Observable>
7 <indicator:Indicated_TTP>
8 <stix:TTP id="ttp-...">
9 <ttp:Behavior>
10 <ttp:Malware_Instance>
11 <ttp:Name> Lazarus </ttp:Name>
12 </ttp:Malware_Instance>
13 <ttp:Behavior>
14 ...
15 </stix:TTP>
16 </indicator:Indicated_TTP>
AlienVault OTX Cyware DigitalSide IBM X−Force JamesBrine 17 </stix:Indicator>
Fig. 8: Example of improper value of an attribute. Note that
Lazarus, a threat actor, is assigned to the Malware Instance
attribute in the TTP object.
Incorrect keywords in attributes. To evaluate the improper
values of attributes, we first focus on the mapping between
attributes and values in three objects – Threat actor, TTP, and
Malware – where values are a form of attribute keywords.
(b) STIX 2. To quantify the level of improper values, we first generate
the ground truth dataset of threat actors and malware fami-
Fig. 7: Average number of attributes used in Indicator objects lies by querying three data sources: Malpedia [16], MITRE
for each source per month. The red zones indicate periods of ATT&CK [47], and MISP GitHub4, which are widely used
rapid increase in attribute usage in Indicator objects, while for labeling threat actors or malware families [67], [59], [32].
the blue zone indicates a period of rapid decrease in attribute In detail, for each STIX data in the datasets, we extract the
usage. values of the Name attributes of the Threat actor, TTP, and
Malware and make a list of names. As there can be different
keywords that indicate the same target (e.g., “Lazarus” and
“HiddenCobra”),wealsorefertothe“alias”fieldsandinclude
VII. QUALITY
all the values of the field in our keyword list. Finally, the list
This section analyzes the quality of STIX data by evaluat- includes 2,111 threat actor names and 4,071 malware family
ing improper values and improper usage of objects/attributes, names. Then, we search all the names in the list of the data
utilizing CTI-Lense. sources and classify the results into one of Threat actor, TTP,
Malware, or Unknown. Also, we collect naming conventions
A. Improper Value of threat actors (e.g., “UNC N”, “APT-C-N,” and “Magecart
groupN”)usedbysecurityvendors.Basedonthegroundtruth
We analyze whether the values in STIX objects and at-
dataset and naming conventions, we determine the mapping
tributeswrittenbyproducersarecorrect.Wefocusspecifically
between attributes and values in STIX data is correct if the
on values in three objects for each STIX version – STIX
mapping matches any entry in the ground truth dataset or a
1: Indicator, TTP, and Threat actor; and STIX 2: Indicator,
value in the Name attribute of the Threat actor follows any of
Malware, and Threat actor.
the naming conventions. For instance, if we find “UNC 10” in
Figure 8 shows an example of an improper value case, the Name attribute of the Threat actor, we consider it correct
where Lazarus, a threat group name, is described with as the name follows the naming convention “UNC N.”
the Name attribute of the TTP object. It is also important
We report our analysis results in Figure 9, where Correct
to ensure the simple hash value in the Observable attribute
means that the values written in the STIX data and in the
contains a malicious file hash value related to Lazarus.
ground truth dataset are in the same objects, or the values
For this example, we examine whether the hash value
follow any of the naming conventions, while Incorrect means
12cc14bbbc421275c3c6145bfa186dff is actually re-
that the values of the STIX data and that of the ground
lated to TTP.
truth dataset are different. We mark as Unmatched the values
In addition, the simple hash value in the Observable at- classified as Unknown in our ground truth dataset. We find
tribute must contain a malicious file hash value, which is that 19% of Threat actor objects of both STIX versions
related to Lazarus, to use STIX properly. Incorrect STIX contain TTP names or Malware names. Furthermore, 62%
datacancausesevereproblems,leadingtoimproperresponses and 58% of malware family information in STIX 1 and 2
to security threats. Rectifying incorrect STIX data can be (i.e., TTP and Malware) does not match any of the entries
a heavy burden for security practitioners who process large
volumes of threat data. 4https://github.com/MISP/misp-galaxy/tree/main/clusters
11

12
TABLE VI: Detection rates of Indicator objects in the STIX dataset by observable type and commercial scanning service.
Detected denotes the proportion of Indicator objects detected by at least one engine out of the total count of requests. Not det.
denotes the proportion of Indicator objects detected by no engine out of the total count of requests. N/A denotes the proportion
of Indicator objects that do not exist in a scanning service.
VirusTotal HybridAnalysis MetaDefender
Observable types Count Detected Not det. N/A Detected Not det. N/A Detected Not det. N/A
AddressObjectType 43,537 50.62% 49.38% 0.00% 8.75% 91.25% 0.00% 30.02% 69.98% 0.00%
DomainNameObjectType 163,121 39.99% 59.90% 0.12% 9.25% 90.63% 0.12% 18.28% 81.59% 0.13%
FileObjectType 88,470 78.37% 1.87% 19.75% 42.24% 5.30% 52.46% 77.02% 3.22% 19.75%
URIObjectType 377,857 97.06% 2.18% 0.76% 2.37% 96.87% 0.77% 91.72% 8.26% 0.02%
Total 672,985 77.77% 19.18% 3.05% 9.69% 82.95% 7.35% 67.99% 29.37% 2.64%
Threat actor (STIX 1)
TTP (STIX 1)
Threat actor (STIX 2)
Malware (STIX 2)
0.0 0.2 0.4 0.6 0.8 1.0
Contained information (%)
stcejbO
XITS
URIObjectType, and AddressObjectType properties
Correct Incorrect Unmatched
of Indicator objects, respectively. Since the STIX data we
searched for may not exist for each service, we measure the
ratio of detected, undetected, and not applicable (N/A) data
for each service.
Table VI summarizes the statistics derived from reports
collected from three services. In total, we query 672,985
Observable data in the Indicator objects and obtain 652,452
(96.95%),623,492(92.65%),and655,213(97.36%)reportsfor
VirusTotal, HybridAnalysis, and MetaDefender, respectively.
We first count the number of detections as the number of
reports that include at least one positive result. Then, we
Fig.9:Proportionofcorrectandincorrectattributevalues.We
compute the detection rate by dividing the detection count by
find that 19% of Threat actor objects of both STIX versions
thenumberoftotalreports.Wefindthat523,352(77.77%)and
contain TTP names or Malware names.
457,595(67.99%)threatsdescribedbythecorrespondingSTIX
data are confirmed by at least one of the engines in Virus-
in the ground truth dataset. To understand the reason for such Total and MetaDefender, respectively. However, only 65,225
unmatched objects, we manually analyzetheir values. We find (9.69%)STIXdataareconfirmedbyHybridAnalysis.Indetail,
that the majority of the values are named based on producers’ the FileObjectType data, which is applicable in each
own conventions. For instance, some STIX producers use source,canbealmostfoundinallthreescanningserviceswith
the substrings (e.g., “DIK” and “ZBOT.DIK”) of the name high accuracy (97.67%, 88.85%, and 95.99% for VirusTotal,
of malware (e.g., “TSPY ZBOT.DIK”) defined by security HybridAnalysis, and MetaDefender, respectively). In addition,
providers (e.g., TrendMicro). There are also some misspelled the URIObjectType data, which is the most dominant
cases(e.g.,“WannaCry”as“Wancry”)thatincreasethenumber observable type (56.15%) in STIX, can be almost found in
of unmatched cases. VirusTotal(99.24%)andMetaDefender(99.98%)withhighac-
curacy(97.80%and91.72%,respectively).However,although
Validationofvaluesinattributes.Weinvestigatewhetherthe most (over 99%) of the AddressObjectType data and
valuesinObservableattributes(e.g.,malwarehashes,domains, DomainNameObjectType data exist in all three scanning
URLs, and IPs), generated between January 1, 2020, and services, their detection rates are low (lower than 50.62%).
January 3, 2022, are actually malicious. On the other hand, the FileObjectType data are the most
missing type in VirusTotal (i.e., up to 52.46% of them are
To build a ground truth dataset of Indicator objects,
missing), but they show the highest accuracy (up to 96.95%).
we retrieve scanning reports from three services: VirusTotal,
HybridAnalysis, and MetaDefender, which are widely used
WeanalyzetheVirusTotaldetectionresultof672,985STIX
to validate threat indicators [28], [38], [37], [21], [55]. For
datainmoredetail.Tothisend,foreachSTIXdata,wecheck
each attribute type (i.e., malware hashes, domains, URLs,
how many detection engines classify it as an anomaly. We
and IP addresses), VirusTotal and MetaDefender provide de-
evaluateaccuracybasedonthethreshold[64],whichindicates
tection results from various anomaly detection engines, and
theminimumnumberofanomalydetectionenginesthatreport
HybridAnalysis provides a threat score with three status tags
apositive.SincethedetectionresultsforindividualVirusTotal
(i.e., malicious, suspicious, and no specific threat). We used
engines can be flipped over time, we measure the accuracy
the report, search, and lookup APIs provided by VirusTotal,
of the data with respect to the various thresholds for each
HybridAnalysis, and MetaDefender, respectively, to obtain the
Observableattributetype.Weselectthresholdsthatrangefrom
latest scanning results. We collected the values for different
t=2 to 39, as suggested by [64].
observable types of indicators, including hashes of malware,
domains,URLs,andIPaddresses,whichcorrespondtotheval- Figure 10 shows that the FileObjectType achieves
uesoftheFileObjectType,DomainNameObjectType, over 90% accuracy when the threshold is set to less than
12

13
1.0
0.8
0.6
0.4
0.2
0.0
0 5 10 15 20 25 30 35 40
Threshold (t)
ycaruccA
Address DomainName File URI
TABLEVII:ValidityofIndicator objectsintheSTIXdataset,
grouped by producer identity, using VirusTotal. Detected de-
notes the proportion of Indicator objects detected by at least
one engine in the VirusTotal.
Producer Observable types # (%) Objects Detected
URIObjectType 328,585 (99.99%) 99.79%
phishtank.com
AddressObjectType - -
URIObjectType - - dshield.org
AddressObjectType 15 (0.00%) 6.67%
URIObjectType 49,963 (14.51%) 77.74%
None AddressObjectType 43,522 (12.64%) 50.63%
Fig. 10: Accuracy for observable types based on each
Total 344,385 (100%) -
threshold t. We note that both FileObjectType and
URIObjectType, which account for 70.34% of all the data,
achieve about 90% of accuracy when the threshold is set to TABLE VIII: Correctly mapped STIX objects with APT re-
lessthanorequaltot=5(96.74%and87.69%,respectively). ports.
AlthoughSTIXdoesnotalwayscontainaccuratevalues,STIX
includes quite verified values for file objects and URIs. Indicator attr. Ref. object Overlap # (%) Correct
Observable Threatactor 17,737 15,875(89.50%)
(STIX1) TTP 10,632 3,601(33.87%)
or equal to t = 20 (90.08% for t = 20). We conclude
that STIX disseminates accurate values, especially for file Pattern Threatactor 27,352 12,538(45.84%)
objects.Subsequently,URIObjectTypeachievesabout90% (STIX2) Malware 1,661 86(5.18%)
accuracy in a small range with a threshold less than or equal
to t = 5 (87.69% for t = 5). However, the detection rate
However, in the case of AddressObjectType objects, the situ-
becomes 0, when the threshold is set to over t = 20. In
addition, we find that about 50% of AddressObjectType ation is reversed. The 15 objects on dshield.org have a
and DomainObjectType data are detected by no engine in very low accuracy of 6.67%, which is significantly lower than
VirusTotal. It means that for the AddressObjectType and objects without producer names (50.63%). We believe that the
DomainObjectType data, STIX data are not confirmed to AddressObjectTypeobjectsmaygenerallyhaveincorrect(orno
longervalid)information,astheobjectsareusedtospecifyIP
be accurate with regard to VirusTotal; however, it does not
addresses, and IP addresses are volatile and temporarily used
mean that the STIX data are inaccurate because there can be
formaliciouspurposes.Also,thiscaseisdifficulttogeneralize,
some addresses or domain names that the VirusTotal engines
as the sample size is only 15.
do not detect. Interestingly, when t = 20, the accuracy of
most observable types becomes 0, but FileObjectType
Correctness of attributes mapping. Finally, we validate the
shows significantly high accuracy. The result indicates that
valuesofObservableattributessuchasThreatactor,TTP,and
STIX provides verified information on file objects.
Malware. For example, in Figure 8, we examine whether the
Validation of values in attributes by producer. We were valueofTTP(12cc14bbbc421275c3c6145bfa186dff)
curioustoseeifthevalidityofSTIXdatavariesdependingon is truly linked to Lazarus.
the producer. To this end, we divided the Indicator objects
To conduct this analysis, it is necessary to verify the
by producer and compared them with VirusTotal detection
validity of each Observable attribute value. We use threat
results. Table VII compares the validity of STIX Indicator
reports written by security experts for the ground truth. In
objects shared on VirusTotal from January 2020 to January
other words, we verify the validity of an Observable (STIX
2022 by producer. We categorized the Indicator objects into
1) or a Pattern (STIX 2) attribute based on the analysis
three groups: those from producers with known identities
resultspresentedinthesethreatreports.Wecollect4,042threat
(phishtank.com and dshield.org) and those without
reports from renowned security companies such as Kaspersky
producer information (‘None’).
and TrendMicro. These reports contain a total of 57,382 Ob-
Our results are based on VirusTotal scanning reports of servable (STIX 1) or Pattern (STIX 2) attribute values within
672,985 Indicator objects shared over two years. Of the Indicator objects that overlap with our dataset. Table VIII
328,600 (48.83% out of 672,985) Indicator objects with shows the number and proportion of valid attribute values
producer names, 328,585 objects (99.99%) are produced by verifiedbythethreatreports.Interestingly,approximately90%
phishtank.com and achieve a very high accuracy of of Threat actor attributes in STIX 1 are valid. However, for
99.79%, which is much higher than URIObjectType objects TTP attributes in STIX 1, only about 34% have valid values.
without producer names (77.74% out of 49,963)5. This sug- Furthermore, the rate of valid attribute values for Threat
geststhattheaccuracyofURIObjectTypeobjectswithproducer actorandMalwareattributesinSTIX2isrelativelylower,
names is higher than that of objects without producer names. standing at 45.84% and 5.18%, respectively.
5Notethatatotalnumberofobjectswithoutproducersis344,385(51.17% Theresultsofouranalysishighlightasubstantialmismatch
outof672,985),butonly49,963(14.51%)hasURIObjectTypeobjects. between the attributes of STIX data, excluding Threat Actor,
13

14
1 <stix:STIX_Package ...>
2 <stix:STIX_Header> 160
3 <stix:Title> Dtrack and ATMDtrack ATM
140
Malware Linked to Lazarus </stix:Title>
4 <stix:Description> Summary After discovering 120
ATM malware they named ATMDtrack, Kaspersky 100
found a significant number of other related 80
samples of malware which they have named 60
Dtrack ... A remote access Trojan (RAT) is 40
also installed ... </stix:Description> 20
5 ... 0
6 </stix:STIX_Header> AP MI TA TI AP MI TA TI
7 <stix:Indicators>
Information type (STIX 1) Information type (STIX 2)
8 <stix:Indicator id="...">...</stix:Indicator
>
9 ...
10 </stix:Indicators>
11 </stix:STIX_Package>
Fig. 11: Example of improper usage of an attribute.
ATMDtrack, the name of malware, is described in the De-
scription attribute rather than the Malware Instance attribute.
inSTIX1andtheIoCsreportedinthreatreports.Thediscrep-
ancybetweenSTIXdataandreal-worldthreatreportssuggests
that we need better data quality control mechanisms. This
includesstandardizedprocedures,datavalidation,andongoing
monitoring. We may also need improved methodologies for
generating STIX attributes.
B. Improper Usage
Wemeasurethenumberofobjectsthatcontaininformation,
which can be precisely represented with other objects or
attributes, within a sentence, but does not include the objects
or attributes. For instance, Figure 11 presents an example of a
STIX header containing a narrative description of a malicious
sample, which we defined as improper usage. Specific actions
suchasDtrack,ATMDtrack,andRATaredescribedinTitle
and Description but not in a TTP object that is defined to
describe such actions. Similarly, the threat actor Lazarus
should be represented using the Threat actor object, but it
is only described in Title of STIX Header. We also find this
practice in the STIX 2 dataset. Many STIX 2 data (e.g.,
threat actor and malware) use the description attribute in
Report objects to describe specific information in a narrative
way. Although such attribute usage in Title or Description
does not violate the standard, this practice limits the main
purpose of automatic processing and rapid response to threats
bymachinesasintendedbySTIX.Tothisend,wefirstmakea
listofkeywordsfromAttackpattern,Malwareinstance,Target
information,andThreatactor.Wenextreviewsentencesofthe
Description attribute of the Indicator objects and collect the
objects that include any keyword in the list. From the objects,
we classify an object in which the keyword is not described
with the precise objects or attributes as improper usage.
We make a list of keywords based on the list of keywords
used in the improper usage and the vocabularies, which are
pre-defined keywords for specific attributes listed in the STIX
standard6. We also add all the words used in the Attack
patternobjects(e.g.,BruteForce)andtheTargetinformation
6https://stix.mitre.org/XMLSchema/default vocabularies/1.2.0/stix default
vocabularies.xsd
)k01(
stcejbo
rotacidnI
#
Information in Description Object/Attr
Fig.12:Numberofindicatorobjectswhereinformationiswrit-
ten in the Description attribute and the precise object/attribute
for four information types: Attack pattern (AP), Malware
instance(MI),Threatactor (TA),andTargetinformation(TI).
All the types of the Indicator objects but Attack pattern
imprecisely use attributes in STIX 1 and 98% of the Indicator
objects in STIX 2 include information about attack patterns
only in the Description attribute.
objects (e.g., User information or Google Inc.) as their values
are simple keywords. With the list, we check the Indicator
objects if they are improperly used.
Our analysis shows that all the types of the Indicator
objects but Attack pattern improperly use attributes in STIX 1
(seeFigure12).Onlylessthan45%ofIndicatorobjects,which
include keywords in the description sentence, precisely repre-
sent the information with objects or attributes for all types.
Furthermore, 98% of the Indicator objects in STIX 2 describe
attack pattern information only in a narrative way. The result
shows that producers rely on description rather than strictly
using the specified attributes in describing the threat. Notice
thatthispracticeunderminesmachinereadability,whichisone
of the main purposes of STIX.
Takeaway: Our analysis shows that producers do not prop-
erlyuseobjectsorattributesastheyareintendedintheSTIX
standard. We report two common misusages. First, STIX
does not always contain accurate values, although STIX
includes quite verified values for file objects and URIs.
Second, producers often do not properly select objects and
attributestorepresentvalues.Instead,theydescribespecific
information in a narrative way, rather than using specific
STIX attributes.
VIII. DISCUSSION
In this section, we discuss the impact of our findings and
recommendations to move forward.
A. Impact of Findings
Our findings have several important implications for the
use of STIX data in cybersecurity.
Volume. The low volume of STIX data generated and shared
daily (an average of only 2,063 unique STIX objects) sug-
gests that security service providers have not yet widely
adoptedSTIX.Thisisasignificantlimitation,ascybersecurity
professionals do not have enough STIX data to meet their
14

15
needs.Additionally,thehighduplicationrateamongSTIXdata Automated tools. Our analysis shows that many STIX data
objects suggests that it is essential to remove duplicate STIX contain common errors, such as spelling mistakes, imprecise
data. object and attribute usage, and duplicate data. To detect and
reduce these errors, service providers could deploy automated
Timeliness. The prompt sharing of URL objects is a positive
verification and deduplication processes. Alternatively, spe-
finding,suggestingthatSTIXcanbeeffectivelyusedtodetect
cialized software could be developed and distributed to help
new attack websites. However, the slower sharing of file-type
producers generate structured STIX data from raw sources.
threat objects is a concern, suggesting that STIX is not yet
Such tools can help security analysts avoid human errors and
used effectively to prevent malware attacks.
inconsistencies in STIX objects and attributes during the data
Coverage. The limited number of STIX data types used in generation process.
practice suggests that the STIX standard is underutilized, po-
Accountability. One way to enhance the trustworthiness of
tentiallyduetoitscomplexity.Thisisamissedopportunity,as
STIX data is by upholding the reputation of security analysts
STIXcanrepresentawiderangeofsophisticatedcybersecurity
who produce this data. Currently, anyone can anonymously
threats. The lack of security rules in STIX indicators is also
createandmodifySTIXdata.AlthoughtheProducer attribute
a significant limitation, as it hinders the automation of cyber
is specified in the Indicator object of the STIX standard, it is
attack detection.
included in only 51.28% of STIX 1 data. The integrity of this
Quality. Incorrect threat actor attribution and the frequent use attributeisnotensured,complicatingthetaskoftracingtheau-
ofnarrativetextinattackpatternobjectscanleadtoineffective thorsoftheSTIXdata.AstheresultsinTableVIIdemonstrate,
countermeasures[69]andmakeautomaticprocessingdifficult. STIX objects that include producer names tend to be more
This requires security analysts to manually verify the validity accurate.Standardorganizationsmightconsiderpromotingthe
of CTI data, which is a time-consuming task that requires use of the Producer attribute to motivate STIX producers
security knowledge and experience. For example, a previous to explicitly add their identities. Furthermore, introducing
study [50] showed that three full-time annotators spent five a new attribute representing the signature of the identities
months labeling 133 reports. Such extensive manual efforts to ensure the integrity of the STIX producers’ identities is
can delay timely responses to cyber threats [52]. essential.Maintainingandtrackinghistoricalrecordsregarding
thevalidityofsharedSTIXdatacanmotivatesecurityanalysts
B. Recommendations to produce more reliable data.
Ourfindingssuggestseveralactionsorganizationscantake
to promote the adoption of STIX:
C. Limitations
Common vocabulary. The STIX standard includes several
First, although we attempted to collect data from all the
predefined vocabularies to describe information related to
TAXII servers and public repositories listed in the STIX
STIXobjects.Forexample,theTTPobjectincludesatypeat-
official website [43], we only found ten available STIX data
tributethatrepresentsmalwaretypes,using18keywords(e.g.,
sources. It may introduce some biases in our analysis, and
Adware, Bot, and Ransomware). However, our analysis
more STIX data may help for generalization. We excluded
revealedthatonlytwokeywords(Bot LoaderandRemote
commercial and industrial resources of STIX data because we
Access Trojan) were observed in the collected TTP ob-
set up our scope on publicly available STIX data. As future
jects. Instead of using existing keywords, many producers
work, it would be interesting to conduct a study analyzing
use their own words to describe threat information, as shown
the differences and similarities between the public STIX and
in previous work [58], [59]. This leads to confusion among
commercial and industrial datasets.
consumersandmakessecurityrule-generationtasksespecially
difficult to automate. To address this problem, the security Second, we conducted a keyword-based analysis, which
community should conduct further analysis to represent and may not be accurate, in finding the imprecise usage of the
organize such information effectively. Additionally, we must keywordsinnarrativedescriptions.Toverifythevalidityofthe
bring together a consortium of organizations using STIX to keyword-based analysis, we randomly selected 100 narrative
build a universal and comprehensive standardized vocabulary descriptions and evaluated the imprecise usage of keywords
forCTI.Specifically,similartohownewvulnerabilitiesreceive (seeFigure12).Then,wemanuallycheckedthatthekeywords
new identifiers in databases such as CVE, a public database were correctly used in the description, and our approach
should also be developed for malware. achieved 87% accuracy. However, more effective methods
could be applied to properly extract related information from
Training program. Recall that there are various objects and
narrative descriptions, such as topic modeling in text analysis.
attributes specified in the STIX standard, but only 75% (STIX
1)and75%(STIX2)ofobjectsandlessthan70%ofattributes
Third, CTI-Lense analyzes the generation and dissemina-
areusedformostobjects.Furthermore,ouranalysisshowsthat
tionofSTIXdata.However,itdoesnottracktheadoptionand
producers often improperly use objects and attributes when
utilizationoftheSTIXdatabyvariouscompanies.Theempha-
describing threats, indicating that producers and consumers
sis is primarily on determining the types and extent of STIX
still need to fully benefit from the proper expressiveness of
data shared across platforms. The subsequent consumption of
STIX. Organizations for standardization (e.g., OASIS) and
these data remains unexamined for future work.
service providers need to consider developing educational
programs to train security analysts who use STIX data. They Lastly,thelackofgroundtruthisoneofthemostchalleng-
arerecommendedtoofferpracticalguidelinesforcreatingand ingissuesinouranalysis.Toobtainmorereliableandaccurate
sharing CTI data in the STIX and TAXII standards. actual security threat data, we use at least three representative
15

16
services related to each measurement (e.g., VirusTotal, Hybri- shared during security incidents, there is still room for im-
dAnalysis, and MetaDefender for the timeliness analysis) as provement in practice to fully benefit from STIX’s diverse
sources of security threat data instead of relying on a single expressivepowerandthequalityofvaluesinSTIXdata.Based
service.Notethatthesesourcesarewidelyusedasgroundtruth on our findings in the collected STIX dataset, we suggest
in prior works [28], [38], [37], [21], [55], [67], [59], [32]. four practical recommendations: (1) establishing a common
vocabulary to ensure the consistency and comparability of
IX. RELATEDWORK STIX data across different entities; (2) developing training
programs to encourage security analysts to generate more
Quality and applicability assessment of CTIs. Existing
valid and useful STIX data; (3) using tools that automatically
research has not focused much on analyzing the quantity and
generateSTIXdatafromrawdataornaturallanguagereports,
quality of CTI data. Li et al. [26] conducted a quantitative
and validate the objects and attributes in STIX data; and (4)
evaluation of the coverage and accuracy of various IoC data,
maintaining and tracking the historical records on the validity
finding significant overlap in threat intelligence data from
ofsharedSTIXdatatoencouragesecurityanalyststogenerate
variouspublicsources.Similarly,Griffioenetal.[27]assessed
more credible and trustworthy data.
the timeliness, sensitivity, originality, and impact of 1.38
million indicators from threat intelligence feeds, highlighting
varying response times, with some feeds providing indicators ACKNOWLEDGMENT
within days of a security incident and others lagging behind
The authors would thank anonymous reviewers and the shep-
by months. Bouwman et al. [73] emphasized the practical
herd. Hyoungshick Kim is the corresponding author. This work
effectiveness of publicly available CTI data, showing that
was supported by Korea Internet & Security Agency (KISA) grant
blocklists from public CTI data sources were more successful
(No.1781000003, Development of a Personal Information Protection
at thwarting COVID-19 related phishing attacks than existing
Framework for Identifying and Blocking Trackers) and Institute for
defenses. However, these studies focus on a limited range of
Information & communication Technology Planning & Evaluation
threat intelligence types, such as simple IoC types (IPs and
(IITP) grant funded by the Korea government (NIST) (No.2018-0-
malware hashes) [26], [27] or a specific threat intelligence
00532, Development of High-Assurance (>=EAL6) Secure Micro-
source [73]. Additionally, they do not consider any advanced
kernel, 40%), (No.2019-0-01343, Regional Strategic Industry Con-
threat data, such as TTPs and threat actors. Unlike these
vergence Security Core Talent Training Business), and (No.2022-
existingstudies,wefocusedourresearchonhowSTIXobjects,
0-00495, On-Device Voice Phishing Call Detection). This work
a type of structured CTI data, are being shared.
was also supported by National Science Foundation (2210137 and
Automated extraction and generation of CTIs. Several 2335798),ScienceAlliance’sStARTprogramandgiftsfromGoogle
attempts have been made to automate the extraction and exploreCSR and TensorFlow, and the KENTECH Research Grant
generation of structured CTI data from unstructured data. (202200048A).
For instance, Kim et al. [36] introduced CyTIME, a CTI
managementframeworkthatintegratesheterogeneousCTIdata
REFERENCES
into a single, structured STIX format and auto-generates se-
curity rules. Sadique et al. [22] proposed a privacy-preserving [1] Abnormal, “Fraudsters use email in phone fraud scams,
mechanism that utilizes Syslog data and represents raw cyber targeting 89% of organizations,” 2021, access date: 28 July
2022. [Online]. Available: https://medcitynews.com/uploads/2022/03/
threat data in STIX format in an automated manner. However,
Abnormal-Security H2-2021 Email-Threat-Report.pdf
these frameworks generate structured CTI data that contains
[2] Accenture, “The cost of cybercrime,” 2019, access date: 28 July
onlysimpleIoCinformation.Toaddressthislimitation,several 2022.[Online].Available:https://www.accenture.com/ acnmedia/PDF-
frameworkshavebeenproposedtogeneratestructureddata,in- 96/Accenture-2019-Cost-of-Cybercrime-Study-Final.pdf
cludingadvanceddatasuchasthreatactorsandIoCcategories, [3] R. Andrew, S. Stavros, and K. Nicholas, “A comparative analysis of
fromunstructureddata.Forexample,Kimetal.[14]proposed cyber-threat intelligence sources, formats and languages,” Electronics,
CTIMiner, which parses public security reports and generates vol.9,p.824,2020.
structured CTI data for specific domains (e.g., finance and [4] M. Antonakakis, T. April, M. Bailey, M. Bernhard, E. Bursztein,
J.Cochran,Z.Durumeric,J.A.Halderman,L.Invernizzi,M.Kallitsis
IoT). Similarly, Koloveas et al. [49] proposed INTIME to
et al., “Understanding the mirai botnet,” in Proceedings of the 26th
identify and analyze cyber threats in the IoT domain. Zhao et
USENIXSecuritySymposium,2017,pp.1093–1110.
al. [34] proposed machine learning-based methodologies for
[5] S.Appala,N.Cam-Winget,D.McGrew,andJ.Verma,“Anactionable
creating highly accurate CTI data in an automated manner, threat intelligence system using a publish-subscribe communications
leveraging Convolutional Neural networks (CNNs), word em- model,” in Proceedings of the 2nd ACM Workshop on Information
bedding, and syntactic dependency. Fujii et al. [63] proposed SharingandCollaborativeSecurity,2015,pp.61–70.
a method to generate CTI data using several state-of-the- [6] AV-TEST, “Malware statistics,” 2023, access date: 15 May 2023.
[Online].Available:https://www.av-test.org/en/statistics/malware/
art natural language processing techniques, such as named
entity recognition and relation extraction, to extract named [7] H.Booth,D.Rike,andG.A.Witte,“Thenationalvulnerabilitydatabase
(NVD):Overview,”2013.
entities and relations between them. Despite these efforts to
[8] M.Botacin,F.Ceschin,P.DeGeus,andA.Gre´gio,“Weneedtotalk
generatesophisticatedstructuredCTIdata,thebest-performing
about antiviruses: challenges & pitfalls of av evaluations,” Computers
frameworks still achieve F1 scores of around 80%. &Security,vol.95,p.101859,2020.
[9] J. Bret, P. Rich, and D. Trey, “OASIS Standard - STIX Version
X. CONCLUSION 2.1,” 2022, access date: 15 December 2022. [Online]. Available:
https://docs.oasis-open.org/cti/stix/v2.1/os/stix-v2.1-os.pdf
In this paper, we analyze STIX usage in terms of volume,
[10] R. Brown and R. M. Lee, “The evolution of cyber threat intelligence
timeliness,coverage,andquality.AlthoughSTIXisfrequently (CTI):2019sansctisurvey,”SANSInstitute,2019.
16

17
[11] M. Corporation, “Cyber observable eXpression (CybOX),” 2017, [32] R. J. Joyce, D. Amlani, C. Nicholas, and E. Raff, “MOTIF: A mal-
access date: 23 May 2022. [Online]. Available: http://cyboxproject. warereferencedatasetwithgroundtruthfamilylabels,”Computers&
github.io/releases/2.1/ Security,vol.124,p.102921,2023.
[12] CrowdStrike, “Hybrid Analysis,” 2012, access date: 27 May 2022. [33] C.Julie,D.Mark,andS.Charles,“Thetrustedautomatedexchangeof
[Online].Available:https://www.hybrid-analysis.com/ indicatorinformation(TAXII),”MITRECorporation,pp.1–20,2014.
[13] W. Cynthia, D. Alexandre, W. Ge´rard, and I. Andras, “Misp: The [34] Z.Jun,Y.Qiben,L.Jianxin,S.Minglai,H.Zuti,andL.Bo,“TIMiner:
designandimplementationofacollaborativethreatintelligencesharing Automatically extracting and analyzing categorized cyber threat intel-
platform,”inProceedingsofthe3rdACMonWorkshoponInformation ligence from social data,” Computers & Security, vol. 95, p. 101867,
SharingandCollaborativeSecurity,2016,pp.49–56. 2020.
[14] K.DaegeonandK.H.Kang,“Automateddatasetgenerationsystemfor [35] M. K. and S. Khandelwal, “The hacker news,” 2010, access date: 15
collaborative research of cyber threat analysis,” Security and Commu- May2023.[Online].Available:https://thehackernews.com/
nicationNetworks,vol.2019,2019.
[36] E. Kim, K. Kim, D. Shin, B. Jin, and H. Kim, “CyTIME: Cyber
[15] N. Daniel, “A behavioural-based approach to ransomware detection,” threatintelligencemanagEmentframeworkforautomaticallygenerating
Whitepaper.MWRLabsWhitepaper,2017. securityrules,”inProceedingsofthe13thInternationalConferenceon
FutureInternetTechnologies,2018,pp.1–5.
[16] P. Daniel, C. Martin, E. Steffen, and P. Elmar, “Malpedia: a collab-
orative effort to inventorize the malware landscape,” The Journal on [37] Z.Li,Q.A.Chen,C.Xiong,Y.Chen,T.Zhu,andH.Yang,“Effective
Cybercrime&DigitalInvestigations,2017. and light-weight deobfuscation and semantic-aware attack detection
for powershell scripts,” in Proceedings of the 26th ACM SIGSAC
[17] S. Daniel, B. Fabian, C. Marco, and P. Gu¨nther, “Measuring and
Conference on Computer and Communications Security, 2019, pp.
visualizing cyber threat intelligence quality,” International Journal of
1831–1847.
InformationSecurity,vol.20,pp.21–38,2021.
[38] Y. Lin, R. Liu, D. M. Divakaran, J. Y. Ng, Q. Z. Chan, Y. Lu, Y. Si,
[18] S. Dave, “Cyber threat intelligence uses, successes and failures: The
F. Zhang, and J. S. Dong, “Phishpedia: A hybrid deep learning based
SANS2017CTIsurvey,”SANSInstitute,2017.
approachtovisuallyidentifyphishingwebpages,”inProceedingsofthe
[19] deMeloeSilvaAlessandra,C.G.J.Jose´,deOliveiraAlbuquerqueRob- 30thUSENIXSecuritySymposium,2021,pp.3793–3810.
son, and G. V. L. Javier, “A methodology to evaluate standards and
[39] Malware Must Die, “Mmd-0056-2016 - linux/mirai, how an old elf
platformswithincyberthreatintelligence,”FutureInternet,vol.12,p.
malcode is recycled,” 2016, access date: 15 May 2023. [Online].
108,2020.
Available: https://blog.malwaremustdie.org/2016/08/mmd-0056-2016-
[20] Deep-Instinct, “Cyber threat landscape report,” 2020, linuxmirai-just.html
access date: 28 July 2022. [Online]. Avail-
[40] Y.MerahandT.Kenaza,“Ontology-basedcyberriskmonitoringusing
able: https://info.deepinstinct.com/en/tof/cyber-threat-report-2021?
cyber threat intelligence,” in Proceedings of the 16th International
ga=2.149360946.587810922.1658982278-1327454874.1658982278
ConferenceonAvailability,ReliabilityandSecurity,2021,pp.1–8.
[21] P. Dodia, M. AlSabah, O. Alrawi, and T. Wang, “Exposing the rat in
[41] L. Metcalf and J. M. Spring, “Blacklist ecosystem analysis: Spanning
thetunnel:UsingtrafficanalysisforTor-basedmalwaredetection,”in
jan 2012 to jun 2014,” in Proceedings of the 2nd ACM Workshop on
Proceedings of the 29th ACM SIGSAC Conference on Computer and
InformationSharingandCollaborativeSecurity,2015,pp.13–22.
CommunicationsSecurity,2022,pp.875–889.
[42] Microsoft, “Threat intelligence integration in microsoft sen-
[22] S. Farhan, C. Sui, V. Iman, B. Shahriar, and S. Shamik, “Automated
tinel,” 2023, access date: 12 May 2022. [Online]. Avail-
structured threat information expression (STIX) document generation
able: https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/
withprivacypreservation,”inProceedingsofthe9thIEEEAnnualUbiq-
sentinel/threat-intelligence-integration.md
uitous Computing, Electronics & Mobile Communication Conference,
2018,pp.847–853. [43] MITRE Corporation, “STIX/TAXII Supporters List,” 2014, access
date:23November2021.[Online].Available:https://stixproject.github.
[23] P.ForemskiandP.Vixie,“Themodalityofmortalityindomainnames,”
io/supporters/
Virus,p.1,2018.
[44] S. Mohurle and M. Patil, “A brief study of wannacry threat: Ran-
[24] S. Gallagher, “Locky: crypto-ransomware rides in
somwareattack2017,”InternationalJournalofAdvancedResearchin
on malicious word document macro,” 2016, ac-
ComputerScience,vol.8,no.5,pp.1938–1940,2017.
cess date: 15 December 2022. [Online]. Avail-
able: https://arstechnica.com/information-technology/2016/02/locky- [45] M.A.Napierala,“Whatisthebonferronicorrection?”AaosNow,pp.
crypto-ransomware-rides-in-on-malicious-word-document-macro/ 40–41,2012.
[25] Y.Ghazi,Z.Anwar,R.Mumtaz,S.Saleem,andA.Tahir,“Asupervised [46] OPSWAT, “MetaDefender Cloud-Advanced threat prevention and
machinelearningbasedapproachforautomaticallyextractinghigh-level detection,” 2012, access date: 27 May 2022. [Online]. Available:
threat intelligence from unstructured sources,” in Proceedings of the https://metadefender.opswat.com/
16thInternationalConferenceonFrontiersofInformationTechnology.
[47] A.Otis,B.Misha,andS.Jacob,“Mitreatt&ck®forindustrialcontrol
IEEE,2018,pp.129–134.
systems:Designandphilosophy,”MitreCorporation,2020.
[26] L. V. Guo, D. Matthew, P. Paul, M. Damon, V. G. M, and S. Stefan,
[48] P. S. PandaLabs, “One third of all computer viruses that
“Readingthetealeaves:Acomparativeanalysisofthreatintelligence,”
exist were created in the first 10 months of 2010,”
in Proceedings of the 28th USENIX Security Symposium, 2019, pp.
2010, access date: 15 May 2023. [Online]. Available:
851–867.
https://www.pandasecurity.com/en/mediacenter/press-releases/one-
[27] G.Harm,B.Tim,andD.Christian,“Qualityevaluationofcyberthreat third-of-all-computer-viruses-that-exist-were-created-in-the-first-10-
intelligencefeeds,”inProceedingsofthe18thInternationalConference months-of-2010/
onAppliedCryptographyandNetworkSecurity,2020,pp.277–296.
[49] K.Paris,C.Thanasis,A.Sofia,S.Spiros,andT.Christos,“INTIME:
[28] D. Hitaj, G. Pagnotta, B. Hitaj, L. V. Mancini, and F. Perez-Cruz, Amachinelearning-basedframeworkforgatheringandleveragingweb
“MaleficNet:Hidingmalwareintodeepneuralnetworksusingspread- datatocyber-threatintelligence,”Electronics,vol.10,p.818,2021.
spectrumshannelcoding,”inProceedingsof27thEuropeanSymposium
[50] Y. Park and T. Lee, “Full-stack information extraction system for
onResearchinComputerSecurity. Springer,2022,pp.425–444.
cybersecurity intelligence,” in Proceedings of the 27th Conference on
[29] J. Hurley, “Threat post,” 2009, access date: 15 May 2023. [Online]. Empirical Methods in Natural Language Processing: Industry Track,
Available:https://threatpost.com/ 2022,pp.531–539.
[30] IBM, “What is threat hunting?” access date: 22 June 2023. [Online]. [51] J.Pearl,Causality. Cambridgeuniversitypress,2009.
Available:https://www.ibm.com/topics/threat-hunting
[52] Ponemon Institute, “Live threat intelligence impact report 2013,”
[31] M. Jason, “Using network based security systems to search for STIX 2013,accessdate:15May2023.[Online].Available:https://www.ten-
andTAXIIbasedindicatorsofcompromise,”2015. inc.com/presentations/Norse-Ponemon-Report.pdf
17

18
[53] ——,“Thevalueofthreatintelligence:AnnualstudyofNorthAmerican 20th International Symposium on Research in Attacks, Intrusions and
&UnitedKingdomcompanies,”2019. Defenses. Springer,2017,pp.472–493.
[54] G. Ratnam, “Cleaning up solarwinds hack may cost as [73] B. Xander, L. P. Victor, F. Pawel, V. G. Tom, G. C. H, M. Giovane,
much as $100 billion,” 2021, access date: 28 July T. Samaneh, J. Wouter, and van Eeten Michel, “Helping hands: Mea-
2022. [Online]. Available: https://rollcall.com/2021/01/11/cleaning- suringtheimpactofalargethreatintelligencesharingcommunity,”in
up-solarwinds-hack-may-cost-as-much-as-100-billion/ Proceedingsofthe31stUSENIXSecuritySymposium,2022.
[55] A. Salem, S. Banescu, and A. Pretschner, “Maat: Automatically ana-
lyzingvirustotalforaccuratelabelingandeffectivemalwaredetection,”
ACM Transactions on Privacy and Security, vol. 24, no. 4, pp. 1–35,
2021.
[56] C.Sauerwein,C.Sillaber,A.Mussmann,andR.Breu,“Threatintelli-
gencesharingplatforms:Anexploratorystudyofsoftwarevendorsand
researchperspectives,”2017.
[57] B.Sean,“Standardizingcyberthreatintelligenceinformationwiththe
structured threat information expression (STIX),” Mitre Corporation,
vol.11,pp.1–22,2012.
[58] M.Sebastia´n,R.Rivera,P.Kotzias,andJ.Caballero,“Avclass:Atool
formassivemalwarelabeling,”inProceedingsofthe19thInternational
SymposiumonResearchinAttacks,IntrusionsandDefenses,2016,pp.
230–253.
[59] S.Sebastia´nandJ.Caballero,“Avclass2:Massivemalwaretagextrac-
tion from av labels,” in Proceedings of the 36th Annual Computer
SecurityApplicationsConference,2020,pp.42–53.
[60] Securelist by Kaspersky, “Wannacry and lazarus group – the missing
link?” 2017, access date: 15 May 2023. [Online]. Available: https:
//securelist.com/wannacry-and-lazarus-group-the-missing-link/78431/
[61] Securelist by Kaspersky, “Wannacry ransomware used in widespread
attacks all over the world,” 2017, access date: 15 May 2023.
[Online].Available:https://securelist.com/wannacry-ransomware-used-
in-widespread-attacks-all-over-the-world/78351/
[62] A.Seth,“Grangercausality,”Scholarpedia,vol.2,p.1667,2007.
[63] F. Shota, K. Nobutaka, S. Tomohiro, and Y. Toshihiro, “CyNER:
Informationextractionfromunstructuredtextofctisourceswithnon-
contextualiocs,”inProceedingsofthe17thInternationalWorkshopon
Security. Springer,2022,pp.85–104.
[64] Z. Shuofei, S. Jianjun, Y. Limin, Q. Boqin, Z. Ziyi, S. Linhai, and
W. Gang, “Measuring and modeling the label dynamics of online
anti-malware engines,” in Proceedings of the 29th USENIX Security
Symposium,2020,pp.2361–2378.
[65] SIEMENS, “Caught in the crosschairs: Are utilities keeping up with
theindustrialcyberthreat?”2019,accessdate:28July2022.[Online].
Available: https://assets.siemens-energy.com/siemens/assets/api/uuid:
c723efb9-847f-4a33-9afa-8a097d81ae19/siemens-cybersecurity.pdf
[66] H. Slatman, “Awesome-threat-intelligence,” 2015, access date: 23
November 2021. [Online]. Available: https://github.com/hslatman/
awesome-threat-intelligence/
[67] M. R. Smith, N. T. Johnson, J. B. Ingram, A. J. Carbajal, B. I.
Haus, E. Domschot, R. Ramyaa, C. C. Lamb, S. J. Verzi, and W. P.
Kegelmeyer, “Mind the gap: On bridging the semantic gap between
machine learning and malware analysis,” in Proceedings of the 13th
ACM Workshop on Artificial Intelligence and Security, 2020, pp. 49–
60.
[68] K.Thomas,R.Amira,A.Ben-Yoash,O.Folger,A.Hardon,A.Berger,
E.Bursztein,andM.Bailey,“Theabusesharingeconomy:Understand-
ing the limits of threat exchanges,” in Proceedings of the 19th Inter-
national Symposium on Research in Attacks, Intrusions and Defenses.
Springer,2016,pp.143–164.
[69] W. Tounsi and H. Rais, “A survey on technical threat intelligence in
theageofsophisticatedcyberattacks,”Computers&security,vol.72,
pp.212–233,2018.
[70] X.Ugarte-Pedrero,M.Graziano,andD.Balzarotti,“Acloselookata
dailydatasetofmalwaresamples,”ACMTransactionsonPrivacyand
Security(TOPS),vol.22,no.1,pp.1–30,2019.
[71] VirusTotal, “VirusTotal-Free Online Virus, Malware and URL
scanner,” 2012, access date: 27 May 2022. [Online]. Available:
https://www.virustotal.com
[72] T.Vissers,J.Spooren,P.Agten,D.Jumpertz,P.Janssen,M.VanWese-
mael,F.Piessens,W.Joosen,andL.Desmet,“Exploringtheecosystem
ofmaliciousdomainregistrationsinthe.eutld,”inProceedingsofthe
18

19
APPENDIXA D. Major Claims
ARTIFACTAPPENDIX
Our major claims are as follows:
This document describes how to use the source code of
CTI-LENSE, a tool that collects STIX data from a set of open
• (C1): CTI-LENSE shows that STIX data producers ac-
tively generate and disseminate STIX data within 2–12
CTIsourcesandsystematicallyanalyzesthegathereddata.The
days after a security incident. This claim is supported by
codeevaluatestheSTIXdatasetintermsofvolume,timeliness,
Experiment1(E1),whosefindingsaredetailedinSection
diversity, and quality.
5.
• (C2): CTI-LENSE shows that STIX data is mostly based
on Indicator objects, accounting for over 90% of all
A. Description & Requirements
the data. Most of the Indicator objects contain simple
Below we explain about how to set up CTI-LENSE, indicators of compromise information, such as malicious
providing links to get the source code, dataset, and docker filehashorURLstrings.ThisisevidencedbyExperiment
image. The docker image includes all requirements for our 2(E2),whoseresultsarereportedinTableIVandTableV
experiments, such as software, source code, and dataset. So, of Section VI.
we highly recommend you to directly use our docker image. • (C3): CTI-LENSE shows that producers do not always
adhere to the intended use of objects and attributes in
1)Access: the STIX standard. First, STIX does not always contain
accurate values, although it generally includes verified
• Source code (Github): https://github.com/SKKU-SecLab/ valuesforfileobjectsandURIs.Second,producersdonot
CTI Lense. consistently select the appropriate objects and attributes
• Source code (figshare): https://dx.doi.org/10.6084/m9. to represent values. Instead, they often describe specific
figshare.24581004. information in a narrative way, bypassing specific STIX
• Dataset(figshare):https://dx.doi.org/10.6084/m9.figshare. attributes.ThisisevidencedbyExperiment3(E3),whose
24126336. resultsarereportedinFigures9,10,12,andTableVIand
• Docker image: jinbumjin/cti-lense:artifactv1.1 VIII of Section VII.
2)Hardware Dependencies: Our experiments are run on a E. Evaluation
virtual machine with at least 4 cores and 32 GiB memory.
The operational steps and experiments for our evaluation
3)SoftwareDependencies: Weprovidetheenvironmentfor are as follows:
ourexperimentswithsourcecodeusingadockerimage,soyou
1)Experiment 1 (E1): [Timeliness] [1 compute-second]:
must install the docker client.
This experiment shows the causality test results between daily
4)Benchmarks: None. security incidents from Malpedia and the daily shared STIX
data. You can observe that STIX data are disseminated 2—
12 days post the incident reporting on Malpedia, with a
significance level p<0.05.
B. Artifact Installation & Configuration
[Preparation] Ensure that the docker container is running
To begin with, the docker client should be installed on
and the mongodb is enabled in the docker container.
Ubuntu 18.04. Then, download and run the docker image of
CTI-LENSE.Thefollowingcommandswillinstalltherequired [Execution] Run the “CTI Lense.py” script to obtain our
dependencies and pull/run the docker image: experimental results using the command:
$ sudo apt-get install docker.io /CTI_Lense# python3 CTI_Lense.py -e timeliness
$ sudo docker pull jinbumjin/cti-lense:artifactv1.1
$ sudo docker run -it jinbumjin/cti-lense:artifactv1
[Results] The output shows the results of causality tests
.1 /bin/bash
between the daily Malpedia and STIX datasets, for time lags
of 1 to 30 days. The p-values for time lags of 2 to 12 days
are less than 0.0008 (adjusted by Bonferroni correction).
C. Experiment Workflow
2)Experiment 2 (E2): [Diversity] [3 compute-minutes]:
This experiment shows how STIX data objects and attributes
Toreproducetheresultsthatwereportinourpaper,follow
are utilized, as detailed in Table IV of Section VI. It also
the instructions below:
elucidates the attributes used in Indicator objects, as shown in
First,starttheMongoDBserviceinthedockercontainer.It Table V of Section VI.
takes up to 10 minutes to enable the MongoDB service. Then,
[Preparation] Ensure that the docker container is running
to get our experimental results, simply run CTI_Lense.py
and the mongodb is enabled in the docker container.
code with the following commands in the docker container.
[Execution] Run the “CTI Lense.py” script to obtain our
/CTI_Lense# service mongodb start
experimental results using the command:
/CTI_Lense# python3 CTI_Lense.py -e analysis_type
/CTI_Lense# python3 CTI_Lense.py -e diversity
19

20
[Results] The output shows the results of Table IV and
V. Table IV indicates that the Indicator object type is most
frequently employed, accounting for 98.77% in STIX 1 and
94.93% in STIX 2. Table V indicates that a considerable
numberofIndicatorobjectsdonotcontainadvancedattributes.
Specifically, in STIX 1, only 53.73% of the objects have the
attributeType,34.76%haveIndicated TTP,andamere0.09%
haveTest Mechanisms.InSTIX2,only17.13%oftheobjects
have both Indicator types and Kill chain phases.
3)Experiment 3 (E3): [Quality] [1 compute-hour]: This
experiment measures and reports the quality of STIX data,
including improper values and usages, in Figures 9, 10, 12,
and Table VI and VIII of Section VII
[Preparation] Ensure that the docker container is running
and the mongodb is enabled in the docker container.
[Execution] Run the “CTI Lense.py” script to obtain our
experimental results using the command:
/CTI_Lense# python3 CTI_Lense.py -e quality
[Results] The output shows the results of Figures 9, 10,
12, and Table VI and VIII. These results indicate that STIX
dataoftencontainsinaccuratevaluesandnarrativedescriptions
instead of specific STIX attributes.
20
