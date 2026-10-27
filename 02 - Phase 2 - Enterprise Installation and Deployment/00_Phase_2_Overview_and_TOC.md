# 🏗️ Phase 2: Enterprise Installation, Sizing & Deployment Setup

> **The IBM QRadar Administrator's Technical Masterclass**  
> **Timeline:** Days 16–22 | **Focus:** Hardware Sizing, OS Partitioning, Clustering, REST API Security, RBAC & Backup Architecture  
> **Target Audience:** 1.5-Year Administrators $\rightarrow$ Senior SIEM Engineers & Deployment Architects

---

## 📸 Phase 2 Visual Gallery: Enterprise Deployment Architecture

| Hardware Sizing & Partitioning | Console Installation & Deployment |
| :---: | :---: |
| ![Hardware Specs & Partitioning\|500](../assets/20_hardware_specs_partitioning_3d.jpg) | ![Console Installation\|500](../assets/21_console_installation_setup_3d.jpg) |
| *Figure 2.1: Hardware Specs & OS Partitioning (`![[20_hardware_specs_partitioning_3d.jpg|500]]`)* | *Figure 2.2: Console Setup & DB Bootstrapping (`![[21_console_installation_setup_3d.jpg|500]]`)* |

| Distributed Managed Hosts & Key Exchange | Dual-Axis RBAC & Security Profiles |
| :---: | :---: |
| ![Distributed Clustering\|500](../assets/09_distributed_topologies_sizing_3d.jpg) | ![RBAC & Security Profiles\|500](../assets/15_network_hierarchy_domains_3d.jpg) |
| *Figure 2.3: Managed Host Onboarding & SSH 22 (`![[09_distributed_topologies_sizing_3d.jpg|500]]`)* | *Figure 2.4: Dual-Axis RBAC & Data Scopes (`![[15_network_hierarchy_domains_3d.jpg|500]]`)* |

---

## 📚 Phase 2 Table of Contents & Hands-on Lab Matrix

| Chapter | Focus Area & Key Architectural Concepts | Embedded Practice Lab | Status |
| :--- | :--- | :---: | :---: |
| [Chapter 16: Hardware Benchmarks, Appliance Specs & OS Partitioning](file:///home/venom/Documents/Study/Study/Qradar/02%20-%20Phase%202%20-%20Enterprise%20Installation%20and%20Deployment/Chapter_16_Hardware_Benchmarks_Appliance_Specs_and_OS_Partitioning.md) | Appliance models (31xx, 16xx, 15xx, 17xx, 14xx, App Host), Sizing math, IOPS benchmarks, RHEL partition layout (`/store`, `/transient`), JVM allocations. | 🧪 **Lab 16** | ✅ Complete |
| [Chapter 17: Console Installation & Software Deployment](file:///home/venom/Documents/Study/Study/Qradar/02%20-%20Phase%202%20-%20Enterprise%20Installation%20and%20Deployment/Chapter_17_Console_Installation_and_Software_Deployment.md) | Bare metal, VMware Thick Provision Eager Zeroed, Cloud AMIs, NTP precision (drift < 1s), FQDN/DNS pre-flight, `setup` installer, Web UI activation. | 🧪 **Lab 17** | ✅ Complete |
| [Chapter 18: Adding & Configuring Managed Hosts](file:///home/venom/Documents/Study/Study/Qradar/02%20-%20Phase%202%20-%20Enterprise%20Installation%20and%20Deployment/Chapter_18_Adding_and_Configuring_Managed_Hosts.md) | 5-stage onboarding, SSH Port 22 key exchange, host tokens, WAN encryption tunnels, Data Node clustering, `all_servers.sh` orchestration. | 🧪 **Lab 18** | ✅ Complete |
| [Chapter 19: Authorized Services, REST API Security & Tokens](file:///home/venom/Documents/Study/Study/Qradar/02%20-%20Phase%202%20-%20Enterprise%20Installation%20and%20Deployment/Chapter_19_Authorized_Services_REST_API_Security_and_Tokens.md) | REST API `/api/`, Swagger UI `/api_doc`, `SEC` header token security, machine vs user credentials, 3-stage async Ariel search, threat intel ingestion. | 🧪 **Lab 19** | ✅ Complete |
| [Chapter 20: User Administration, RBAC & Security Profiles](file:///home/venom/Documents/Study/Study/Qradar/02%20-%20Phase%202%20-%20Enterprise%20Installation%20and%20Deployment/Chapter_20_User_Administration_RBAC_and_Security_Profiles.md) | Dual-axis authorization (User Roles $\times$ Security Profiles), SAML 2.0 / LDAPS (636), payload obfuscation, MSSP domains, `SIM Audit-2` compliance. | 🧪 **Lab 20** | ✅ Complete |
| [Chapter 21: Enterprise Backup Architecture: Config vs Data Recovery](file:///home/venom/Documents/Study/Study/Qradar/02%20-%20Phase%202%20-%20Enterprise%20Installation%20and%20Deployment/Chapter_21_Enterprise_Backup_Architecture_Config_vs_Data.md) | Config Backups (`/store/backup/` 50-500MB) vs Data Backups (Ariel TBs), nightly automation, offsite NFS/rsync, bare-metal disaster recovery runbook. | 🧪 **Lab 21** | ✅ Complete |
| [Chapter 22: Phase 2 Capstone: Building a Distributed Multi-Tier Lab Deployment](file:///home/venom/Documents/Study/Study/Qradar/02%20-%20Phase%202%20-%20Enterprise%20Installation%20and%20Deployment/Chapter_22_Phase_2_Capstone_Building_a_Distributed_Lab.md) | Multi-tier cluster synthesis (Console 3148 + EP 1648 + Data Node 1428 + EC 1505 + App Host 500), 20-Question Benchmark Exam, Disaster Recovery Drill. | 🧪 **Lab 22** | ✅ Complete |

---

## 🎯 Phase 2 Core Competencies Mastered

By completing Phase 2, you have developed the exact competencies required of a **2-Year Experienced Enterprise QRadar Administrator & SIEM Engineer**:
1. **Mathematical Sizing & Storage Architecture**: Calculating raw ingestion, Ariel compressed volumes, and Lucene index sizing for any EPS workload.
2. **Production Installation & Pre-Flight Hardening**: Guaranteeing sub-second NTP synchronization, reverse DNS integrity, and Thick Eager Zeroed storage performance.
3. **Clustering & Data Node Scaling**: Scaling enterprise retention and parallel search query performance across distributed Data Nodes.
4. **Security Automation via REST API**: Programmatically interacting with QRadar via secure `SEC` token headers and asynchronous search polling.
5. **Enterprise Governance & RBAC**: Restricting sensitive payload visibility and bounding analyst investigations using Network Hierarchy CIDRs and Log Source Groups.
6. **Disaster Recovery SLA Execution**: Standing up a recovered Console and resynchronizing an entire distributed cluster within 30 minutes using Configuration Backups.
