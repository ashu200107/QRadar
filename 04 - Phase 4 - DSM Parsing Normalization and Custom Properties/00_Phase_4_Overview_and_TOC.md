# 🔬 Phase 4: DSM Parsing, Normalization & Custom Properties

> **The IBM QRadar Administrator's Technical Masterclass**  
> **Timeline:** Days 36–45 | **Focus:** DSM Architecture, QID Mapping, Custom Properties (Regex, JSONPath, CEF), ReDoS Prevention & Custom DSMs  
> **Target Audience:** 1.5-Year Administrators $\rightarrow$ Senior SIEM Engineers & Detection Engineering Specialists

---

## 📸 Phase 4 Visual Gallery: Parsing & Normalization Architecture

| DSM In-Memory Normalization Pipeline | QID Map Taxonomy & Resolution |
| :---: | :---: |
| ![Normalization Architecture\|500](../assets/stage5_6_parsing_qid_normalization_3d.jpg) | ![QID Map Architecture\|500](../assets/stage5_6_parsing_qid_3d.jpg) |
| *Figure 4.1: Raw String to Tuple Pipeline (`![[stage5_6_parsing_qid_normalization_3d.jpg|500]]`)* | *Figure 4.2: Event Mapping Triad & Taxonomy (`![[stage5_6_parsing_qid_3d.jpg|500]]`)* |

| Custom Property Lucene Indexing | Regex Engine Performance & ReDoS |
| :---: | :---: |
| ![Lucene Column Indexing\|500](../assets/11_ariel_storage_partitions_lucene_3d.jpg) | ![Regex Diagnostics\|500](../assets/19_system_health_monitoring_runbooks_3d.jpg) |
| *Figure 4.3: Custom Property Columnar Slices (`![[11_ariel_storage_partitions_lucene_3d.jpg|500]]`)* | *Figure 4.4: Ingestion Thread Pool ReDoS Defense (`![[19_system_health_monitoring_runbooks_3d.jpg|500]]`)* |

---

## 📚 Phase 4 Table of Contents & Hands-on Lab Matrix

| Chapter | Focus Area & Key Architectural Concepts | Embedded Practice Lab | Status |
| :--- | :--- | :---: | :---: |
| [Chapter 36: DSM Architecture & The Normalization Engine](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_36_DSM_Architecture_and_The_Normalization_Engine.md) | In-memory 2-stage parsing (`ecs-ec-ingress`), Header vs Payload parsing, Event States (Normalized vs Unknown 38750003 vs Stored 38750002). | 🧪 **Lab 36** | ✅ Complete |
| [Chapter 37: The Normalization Pipeline: Raw String to Security Tuple](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_37_The_Normalization_Pipeline_Raw_to_Security_Tuple.md) | Standard Security Tuple (5-tuple, identity, taxonomy, timestamps), Integer IP conversion, 128-byte Ariel binary record vs raw compressed payload. | 🧪 **Lab 37** | ✅ Complete |
| [Chapter 38: QID Map & Event Mapping Architecture](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_38_QID_Map_and_Event_Mapping_Architecture.md) | QID 32-bit integer taxonomy, High/Low Category tree, Event Mapping Triad (`Event ID + Category + DSM ID`), `qidmap_cli.sh`, 2,000,000+ custom namespace. | 🧪 **Lab 38** | ✅ Complete |
| [Chapter 39: The DSM Editor: Live Ingestion Parsing](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_39_The_DSM_Editor_Live_Ingestion_Parsing.md) | Web UI DSM Editor workspace, live event streaming, regex capture groups (`$1`), Property Overrides ("Override system behavior"), instant visual checkmarks. | 🧪 **Lab 39** | ✅ Complete |
| [Chapter 40: Custom Event Properties: Regex Extraction](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_40_Custom_Event_Properties_Regex_Extraction.md) | Custom Event Properties (CEP), Data Types (Text, Number, IP, Port, Date), Lucene Columnar Indexing in Ariel (10x-50x search acceleration), CRE usage. | 🧪 **Lab 40** | ✅ Complete |
| [Chapter 41: Advanced Custom Properties: JSON, CEF & AQL](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_41_Advanced_Custom_Properties_JSON_CEF_AQL.md) | JSONPath expressions (`$.userIdentity.arn`) with Jackson parser, CEF key lookups (`cs1`), AQL search-time derivations (`CONCAT()`, `LOWER()`). | 🧪 **Lab 41** | ✅ Complete |
| [Chapter 42: Log Source Extensions (LSX / XML Architecture)](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_42_Log_Source_Extensions_LSX_XML_Architecture.md) | Declarative XML (`<device-extension>`), `<pattern-definitions>`, `<match-group>`, Parsing Order (Pre-DSM override vs Post-DSM gap fill), `xmllint` validation. | 🧪 **Lab 42** | ✅ Complete |
| [Chapter 43: Troubleshooting Unparsed Events: SIM Generic & Stored](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_43_Troubleshooting_Unparsed_Events_SIM_Generic_and_Stored.md) | Top 5 root causes of `SIM Generic (38750002)` & `Unknown (38750003)`, Case-sensitive identifier mismatches, Non-UTF8 charsets (ISO-8859-1), AQL audits. | 🧪 **Lab 43** | ✅ Complete |
| [Chapter 44: Regex Optimization & Catastrophic Backtracking](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_44_Regex_Optimization_and_Catastrophic_Backtracking.md) | ReDoS vulnerability mechanics, NFA combinatorial explosions ($O(2^n)$), Thread pool freezes, 4 Golden Optimization Rules, Atomic character classes. | 🧪 **Lab 44** | ✅ Complete |
| [Chapter 45: Phase 4 Capstone: Building a Custom DSM from Scratch](file:///home/venom/Documents/Study/Study/Qradar/04%20-%20Phase%204%20-%20DSM%20Parsing%20Normalization%20and%20Custom%20Properties/Chapter_45_Phase_4_Capstone_Building_a_Custom_DSM.md) | Complete end-to-end custom DSM construction for in-house banking app (`AcmeBankingCore`), Custom QIDs (2,000,000+), 20-Question Benchmark Exam. | 🧪 **Lab 45** | ✅ Complete |

---

## 🎯 Phase 4 Core Competencies Mastered

By completing Phase 4, you have mastered the complete parsing and normalization capabilities of a **Senior QRadar SIEM Detection & Parsing Engineer**:
1. **Full Custom DSM Engineering**: Creating new device types, header parsers, payload regex matchers, and custom QIDs from scratch.
2. **QID Taxonomy Mastery**: Resolving events using the Triad (`Event ID + Category + DSM ID`) and creating non-conflicting QIDs in the `2,000,000+` range.
3. **Structured Telemetry Ingestion**: Extracting complex nested cloud objects via JSONPath expressions with zero regular expression overhead.
4. **Lucene Index Optimization**: Selectively indexing high-frequency custom properties to accelerate AQL search queries by up to 50x.
5. **ReDoS Defense & Regex Hardening**: Eliminating catastrophic backtracking loops to ensure sub-microsecond parsing and 100% thread pool health.
6. **Unparsed Event Triage**: Systematically investigating `SIM Generic (38750002)` and `Unknown (38750003)` events to maintain a $> 98\%$ normalized parsing SLA.
