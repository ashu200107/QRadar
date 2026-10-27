# 📚 Phase 1: QRadar Fundamentals & Architecture — Complete Handbook & Table of Contents

> **The IBM QRadar Administrator's Definitive Handbook**  
> **Target Role:** Enterprise QRadar Administrator / SIEM Engineer / SOC L2-L3  
> **Structure:** Zero-Assumption Foundations $\rightarrow$ Production SOC Architecture $\rightarrow$ Linux Internals $\rightarrow$ Practice Labs $\rightarrow$ Interview Drills

---

## 📸 Phase 1 Visual Architecture Gallery

### Enterprise Topology, Pipelines & Sockets
| 1. Enterprise 3D Topology | 2. 8-Stage Ingestion Pipeline Overview |
| :---: | :---: |
| ![3D Architecture|500](../assets/01_enterprise_architecture_3d.jpg) | ![8-Stage Pipeline|500](../assets/02_data_pipeline_8stages_3d.jpg) |
| *Figure 1: Full Multi-Tier Distributed Topology (`![[01_enterprise_architecture_3d.jpg|500]]`)* | *Figure 2: End-to-End Pipeline from Socket to Ariel (`![[02_data_pipeline_8stages_3d.jpg|500]]`)* |

| Stage 1: Log Transport & Ingestion | Stages 2 & 3: Sockets & Spooling Buffer |
| :---: | :---: |
| ![Stage 1: Transport|500](../assets/stage1_log_transport_3d.jpg) | ![Stages 2 & 3: Spooling|500](../assets/stage2_3_ingestion_spooling_3d.jpg) |
| *Figure 3: Multi-Protocol Network Ingestion (`![[stage1_log_transport_3d.jpg|500]]`)* | *Figure 4: `ecs-ec-ingress` & `/store/transient` (`![[stage2_3_ingestion_spooling_3d.jpg|500]]`)* |

| Kernel Socket Ingestion & Spooling Blueprint | Stage 4: Token Bucket License Limiter |
| :---: | :---: |
| ![Socket Ingestion Blueprint|500](../assets/08_qradar_spooling_buffer_pipeline_3d.jpg) | ![Stage 4: Token Bucket|500](../assets/stage4_token_bucket_limiter_3d.jpg) |
| *Figure 4B: NIC DMA to In-Memory & NVMe Spool (`![[08_qradar_spooling_buffer_pipeline_3d.jpg|500]]`)* | *Figure 5: EPS Token Replenishment Valve (`![[stage4_token_bucket_limiter_3d.jpg|500]]`)* |

| Stages 5 & 6: DSM & QID Normalization | Stage 7: CRE Real-Time Correlation Core |
| :---: | :---: |
| ![Stages 5 & 6: Normalization|500](../assets/stage5_6_parsing_qid_normalization_3d.jpg) | ![Stage 7: CRE Engine|500](../assets/stage7_cre_correlation_engine_3d.jpg) |
| *Figure 6: Laser Regex & QID Map Lookup (`![[stage5_6_parsing_qid_normalization_3d.jpg|500]]`)* | *Figure 7: In-Memory Microsecond Rule Evaluation (`![[stage7_cre_correlation_engine_3d.jpg|500]]`)* |

| Stage 8: Magistrate Tower vs. Ariel Vault | 3D Core Mindmap Infographic Diagram |
| :---: | :---: |
| ![Stage 8: Dual Storage|500](../assets/stage8_magistrate_ariel_dual_storage_3d.jpg) | ![Core Mindmap 3D Diagram|500](../assets/07_qradar_core_mindmap_diagram_3d.jpg) |
| *Figure 8: Offense Chaining vs. Columnar Ariel DB (`![[stage8_magistrate_ariel_dual_storage_3d.jpg|500]]`)* | *Figure 9: 3D Core Mindmap Infographic Diagram (`![[07_qradar_core_mindmap_diagram_3d.jpg|500]]`)* |

### Advanced Architecture, Storage & Sizing Visuals
| 3D Distributed Topologies & Sizing (Ch 05) | 3D Daemons & Linux Process Hierarchy (Ch 06) |
| :---: | :---: |
| ![Distributed Topologies|500](../assets/09_distributed_topologies_sizing_3d.jpg) | ![Daemons Hierarchy|500](../assets/10_daemons_process_hierarchy_3d.jpg) |
| *Figure 10: Sizing Models & Multi-Tier Clusters (`![[09_distributed_topologies_sizing_3d.jpg|500]]`)* | *Figure 11: `hostcontext` Master Process Tree (`![[10_daemons_process_hierarchy_3d.jpg|500]]`)* |

| 3D Ariel Storage Vaults & Lucene Indexing (Ch 07) | 3D Magistrate Core & Offense Lifecycle (Ch 08) |
| :---: | :---: |
| ![Ariel Storage Engine|500](../assets/11_ariel_storage_partitions_lucene_3d.jpg) | ![The Magistrate Core|500](../assets/12_magistrate_offense_lifecycle_3d.jpg) |
| *Figure 12: Columnar Records & Super-Intervals (`![[11_ariel_storage_partitions_lucene_3d.jpg|500]]`)* | *Figure 13: Offense Chaining & Magnitude Score (`![[12_magistrate_offense_lifecycle_3d.jpg|500]]`)* |

| 3D Network Ports & Security Matrix (Ch 09) | 3D Licensing Pool & Giveback System (Ch 10) |
| :---: | :---: |
| ![Network Ports Matrix|500](../assets/13_network_ports_security_matrix_3d.jpg) | ![Licensing Giveback Pool|500](../assets/14_licensing_pool_giveback_3d.jpg) |
| *Figure 14: Enterprise Port Matrix (`![[13_network_ports_security_matrix_3d.jpg|500]]`)* | *Figure 15: Dynamic EPS Sharing & Giveback (`![[14_licensing_pool_giveback_3d.jpg|500]]`)* |

| 3D Network Hierarchy & Domains (Ch 11) | 3D Asset Profiler & Identity Model (Ch 12) |
| :---: | :---: |
| ![Network Hierarchy|500](../assets/15_network_hierarchy_domains_3d.jpg) | ![Asset Profiler|500](../assets/16_asset_profiler_identity_model_3d.jpg) |
| *Figure 16: Traffic Directionality & Multi-Tenancy (`![[15_network_hierarchy_domains_3d.jpg|500]]`)* | *Figure 17: Passive Fingerprinting & Identity (`![[16_asset_profiler_identity_model_3d.jpg|500]]`)* |

| 3D Vulnerabilities & Threat Intel (Ch 13) | 3D App Host Docker Framework (Ch 14) |
| :---: | :---: |
| ![Threat Intel Ingestion|500](../assets/17_vulnerability_threat_intel_3d.jpg) | ![App Host Docker Engine|500](../assets/18_app_host_docker_framework_3d.jpg) |
| *Figure 18: TAXII/STIX & CVE Asset Scoring (`![[17_vulnerability_threat_intel_3d.jpg|500]]`)* | *Figure 19: Container Pods & Memory Isolation (`![[18_app_host_docker_framework_3d.jpg|500]]`)* |

| 3D System Health & Admin Runbooks (Ch 15) | Events vs Network Flows (QFlow DPI) |
| :---: | :---: |
| ![System Health Runbooks|500](../assets/19_system_health_monitoring_runbooks_3d.jpg) | ![Events vs Flows|500](../assets/04_events_vs_flows_qflow_3d.jpg) |
| *Figure 20: 24/7 Health Monitoring & Runbooks (`![[19_system_health_monitoring_runbooks_3d.jpg|500]]`)* | *Figure 21: Discrete EPS vs Continuous QFlow DPI (`![[04_events_vs_flows_qflow_3d.jpg|500]]`)* |

---

## 📖 Complete Phase 1 Chapter Directory & Hands-on Practice Labs

- 📄 [Chapter 01: What is SIEM & IBM QRadar Overview](./Chapter_01_What_is_SIEM_and_QRadar_Overview.md) *(Includes 3D Mindmap Infographic, Concept Board, Visual Mind Map, Sticky Notes & Flashcards)*
- 📄 [Chapter 02: QRadar Core Architecture & Components Anatomy](./Chapter_02_QRadar_Core_Architecture_and_Components.md) *(Includes 3D Appliance Breakdown, Master Daemons, PostgreSQL vs Ariel, Mind Map & Sticky Notes)*
- 📄 [Chapter 03: The End-to-End 8-Stage Event Pipeline Masterclass](./Chapter_03_The_End_to_End_Event_Pipeline.md) *(Includes 6 Dedicated 3D Stage Visuals, Token Bucket, Normalization, Diagnostics, Mind Map & Sticky Notes)*
- 📄 [Chapter 04: Events vs Network Flows Deep Dive](./Chapter_04_Events_vs_Flows_Deep_Dive.md) *(Includes 3D DPI Visual, QFlow, NetFlow vs IPFIX, SOC Scenarios, Mind Map & Sticky Notes)*
- 📄 [Chapter 05: Distributed Deployment Topologies & Sizing](./Chapter_05_Distributed_Deployment_Topologies_and_Sizing.md) *(Includes 3D Topology Visual, Sizing Formulas, Appliance Matrix, Mind Map, Sticky Notes & **Lab 05**)*
- 📄 [Chapter 06: Internal Daemons & Linux Subsystems](./Chapter_06_Internal_Daemons_and_Linux_Subsystems.md) *(Includes 3D Daemon Hierarchy, `hostcontext`, JVM Tuning, Mind Map, Sticky Notes & **Lab 06**)*
- 📄 [Chapter 07: Ariel Storage Engine & Directory Layout](./Chapter_07_Ariel_Storage_Engine_and_Directory_Layout.md) *(Includes 3D Ariel Vaults, Payloads vs Records, Lucene Indexing, Retention, Sticky Notes & **Lab 07**)*
- 📄 [Chapter 08: The Magistrate Engine & Offense Lifecycle](./Chapter_08_The_Magistrate_and_Offense_Lifecycle.md) *(Includes 3D Magistrate Core, Indexing Keys, Magnitude S/C/R Math, Timeouts, Sticky Notes & **Lab 08**)*
- 📄 [Chapter 09: Enterprise Port Matrix & Network Security](./Chapter_09_Enterprise_Port_Matrix_and_Network_Security.md) *(Includes 3D Port Matrix, Bidirectional Flows, NAT Mappings, Firewalls, Sticky Notes & **Lab 09**)*
- 📄 [Chapter 10: Licensing & Token Bucket Deep Dive](./Chapter_10_Licensing_and_Token_Bucket_Deep_Dive.md) *(Includes 3D Licensing Pool, Token Bucket Math, Giveback Systems, Queueing, Sticky Notes & **Lab 10**)*
- 📄 [Chapter 11: Network Hierarchy & Multi-Tenancy Architecture](./Chapter_11_Network_Hierarchy_and_Multi_Tenancy.md) *(Includes 3D Network Hierarchy, L2R/R2L/L2L Traffic Directionality, Domains, Sticky Notes & **Lab 11**)*
- 📄 [Chapter 12: Asset Profiler & Identity Model](./Chapter_12_Asset_Profiler_and_Identity_Model.md) *(Includes 3D Asset Profiler, Passive QFlow Fingerprinting, DHCP/Kerberos Identity, Blacklists & **Lab 12**)*
- 📄 [Chapter 13: Vulnerability Management & Threat Intelligence](./Chapter_13_Vulnerability_Management_and_Threat_Intelligence.md) *(Includes 3D Threat Intel, Nessus/Qualys CVE Ingestion, TAXII/STIX 2.1 Feeds & **Lab 13**)*
- 📄 [Chapter 14: App Framework & Dedicated App Host Architecture](./Chapter_14_App_Framework_and_App_Host_Architecture.md) *(Includes 3D App Host Docker Engine, UBA, Pulse, Use Case Manager, Sizing & **Lab 14**)*
- 📄 [Chapter 15: System Notifications, Health Monitoring & Runbooks](./Chapter_15_System_Notifications_Health_Monitoring_and_Runbooks.md) *(Includes 3D Ops Center, Top-Bar Alerts, Daily/Weekly/Monthly Runbooks & **Lab 15**)*
- 📄 [Day 01 Detailed Study Notes & Quiz](./Day_01_QRadar_Architecture_and_SIEM_Foundations.md) *(Full Day 1 Lesson, Linux CLI commands, 16-Question Assessment & Interview Question)*
- 🧪 [Lab 01: Architecture & Pipeline Mapping via CLI](../Labs/Lab_01_Architecture_and_Pipeline_Mapping.md) *(Hands-on verification of daemons, listening ports, spool folders & system logs)*

---

## 🎯 Mastery Checklist for Phase 1
- [ ] **Chapter 01:** Articulate the exact difference between Syslog aggregation and SIEM real-time correlation.
- [ ] **Chapter 02:** Memorize all appliance roles (15xx, 16xx, 17xx, 14xx, 31xx) and their primary daemon assignments (`ecs-ec-ingress`, `ecs-ep`, `hostcontext`).
- [ ] **Chapter 03:** Trace a raw Syslog packet through every single stage of the 8-stage pipeline from memory buffer to Ariel storage.
- [ ] **Chapter 04:** Explain why a SOC requires both Events and Flows during an APT or Data Exfiltration investigation.
- [ ] **Chapter 05 & Lab 05:** Calculate WAN bandwidth and Ariel storage retention for any enterprise EPS requirement.
- [ ] **Chapter 06 & Lab 06:** Inspect and troubleshoot QRadar Linux daemons using `systemctl`, `jstack`, and `ps`.
- [ ] **Chapter 07 & Lab 07:** Explore `/store/ariel/`, audit Lucene indexing, and understand the 85%/90%/95% disk watermarks.
- [ ] **Chapter 08 & Lab 08:** Query offenses in PostgreSQL `qsys` and calculate Magnitude ($f(\text{Severity}, \text{Credibility}, \text{Relevance})$).
- [ ] **Chapter 09 & Lab 09:** Audit listening ports, verify Port 32006/32011 connectivity, and inspect TLS certificate chains.
- [ ] **Chapter 10 & Lab 10:** Audit global license pools, explain Token Bucket givebacks, and simulate burst queueing.
- [ ] **Chapter 11 & Lab 11:** Configure Network Hierarchy subnets and explain L2R, R2L, and L2L directionality in CRE rules.
- [ ] **Chapter 12 & Lab 12:** Query asset profiles in PostgreSQL and configure MAC blacklists to prevent NAT gateway corruption.
- [ ] **Chapter 13 & Lab 13:** Ingest TAXII 2.1 threat feeds into in-memory Reference Sets and link CVEs to Asset Models.
- [ ] **Chapter 14 & Lab 14:** Inspect Docker container pods on App Host and manage app memory limits via `qappmanager`.
- [ ] **Chapter 15 & Lab 15:** Execute the 5-Minute Daily Admin Runbook and generate system diagnostic bundles via `get_logs.sh`.
