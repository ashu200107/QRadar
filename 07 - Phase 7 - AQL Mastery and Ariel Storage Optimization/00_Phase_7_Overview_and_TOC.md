# 📜 Phase 7: AQL Mastery & Ariel Storage Optimization Master Roadmap

> **The IBM QRadar Administrator's Technical Curriculum** | Days 66–73  
> **Target Audience:** 1.5-Year Administrators $\rightarrow$ Senior SIEM Engineers & Detection Engineering Specialists  
> **Core Focus:** ANSI-SQL-92 AQL Grammar, Ariel Storage Internals, Lucene Inverted Indexing, Advanced Aggregations, Subqueries & 10 Enterprise Incident Investigation Queries.

---

## 🎨 3D Architecture Visual Gallery: Phase 7 Modules

| 1. AQL Query Planner & Engine | 2. Ariel Storage Partition Architecture |
| :---: | :---: |
| ![AQL Query Engine|500](../assets/qradar_aql_query_engine_3d_1791647183664.jpg) | ![Ariel Storage Partitions|500](../assets/qradar_ariel_storage_partitions_3d_1791647224940.jpg) |
| *Distributed Query Planner over TCP 32011 (`![[qradar_aql_query_engine_3d_1791647183664.jpg|500]]`)* | *Hourly Columnar Partitions & Lucene Trees (`![[qradar_ariel_storage_partitions_3d_1791647224940.jpg|500]]`)* |

| 3. Advanced Forensic Threat Hunting | 4. Full Telemetry & Incident Correlator |
| :---: | :---: |
| ![Forensic Threat Hunting|500](../assets/qradar_incident_investigation_aql_3d_1791647271083.jpg) | ![Threat Correlator|500](../assets/23_threat_correlation_usecases_3d.jpg) |
| *Multi-Screen SOC AQL Investigation Hub (`![[qradar_incident_investigation_aql_3d_1791647271083.jpg|500]]`)* | *End-to-End MITRE Kill-Chain Analytics (`![[23_threat_correlation_usecases_3d.jpg|500]]`)* |

---

## 🧭 Complete Phase 7 Table of Contents & Study Guide

```
┌──────────────────────────────────────────────────────────────────────────────────────────────────┐
│ 📚 PHASE 7 CHAPTER & LAB ROADMAP                                                                 │
├─────┬────────────────────────────────────────────────────────┬───────────────────────────────────┤
│ Day │ Chapter Title & Core Architecture Focus                │ Hands-on Practice Lab             │
├─────┼────────────────────────────────────────────────────────┼───────────────────────────────────┤
│ 66  │ [[Chapter_66_AQL_Core_Foundations_Syntax_Clauses_and_Execution|Chapter 66: AQL Core Syntax & Query Execution]] │ Lab 66: Executing Queries via CLI │
│ 67  │ [[Chapter_67_Advanced_AQL_Aggregations_COUNT_UNIQUECOUNT_SUM_AVG_TOP|Chapter 67: Advanced AQL Aggregations & Grouping]] │ Lab 67: Multi-Dimensional Aggs    │
│ 68  │ [[Chapter_68_AQL_Function_Library_String_Network_Math_Date_Functions|Chapter 68: AQL Function Library (INCIDR, GeoIP)]] │ Lab 68: GeoIP & CIDR Transforms   │
│ 69  │ [[Chapter_69_Advanced_AQL_Correlation_Subqueries_and_Historical_Baselines|Chapter 69: Subqueries & Historical Baselines]] │ Lab 69: First-Time-Seen Hunts     │
│ 70  │ [[Chapter_70_Ariel_Database_Internals_Partitions_Lucene_and_Pruning|Chapter 70: Ariel Database Storage & Retention]] │ Lab 70: Ariel Disk & Retention    │
│ 71  │ [[Chapter_71_Ariel_Index_Management_Custom_Property_Indexing_and_Benchmarks|Chapter 71: Index Management & CEP Indexing]] │ Lab 71: Benchmarking Indexed CEPs │
│ 72  │ [[Chapter_72_Query_Optimization_and_Troubleshooting|Chapter 72: Query Optimization & Troubleshooting]] │ Lab 72: Runaway Search Cancels    │
│ 73  │ [[Chapter_73_Phase_7_Capstone_10_Enterprise_Forensic_AQL_Queries|Chapter 73: Phase 7 Capstone: 10 Forensic Queries]] │ Lab 73: 10-Query Investigation    │
└─────┴────────────────────────────────────────────────────────┴───────────────────────────────────┘
```

---

## 🎯 Key Milestones Achieved in Phase 7

1. **Distributed AQL Execution Mastery**: Deep understanding of how the Console decomposes AQL into AST search trees, dispatches over TCP 32011 to Managed Hosts, and reduces results in RAM.
2. **Columnar Partitioning & Lucene Index Optimization**: Understanding `/store/ariel/events/records/` vs `payloads/`, enabling custom property indexing for $25\times$ to $65\times$ search speedups.
3. **Advanced Threat Hunting Analytics**: Authoring multi-dimensional aggregations, HyperLogLog distinct counting (`UNIQUECOUNT`), subqueries, and First-Time-Seen (FTS) anomaly searches.
4. **Cluster Stability & Query Troubleshooting**: Diagnosing HTTP 504 proxy timeouts, managing `/opt/qradar/conf/ariel_proxy.conf`, and canceling runaway searches gracefully via `cancelSearch.sh`.
5. **10 Production Forensic Queries**: Equipping the SOC with ready-to-run incident investigation queries covering Brute Force, AD Group Tampering, LOLBins, Ransomware Shadow Deletion, and DNS Tunneling.
