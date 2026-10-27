# 📚 Official IBM Documentation & Technical Knowledge Base References

> **Track:** Enterprise IBM QRadar Administrator & SIEM Architect  
> **Source Material:** Official IBM Documentation, IBM Security Support Knowledge Base, IBM Redbooks, and Architecture Whitepapers.  
> **Applicable Versions:** IBM QRadar SIEM 7.3.x, 7.4.x, and 7.5.x (Up to 7.5.0 UP8+)  

---

## 🏛️ Primary IBM Documentation Portals

| Portal / Resource | URL | Focus Areas |
| :--- | :--- | :--- |
| **IBM Documentation: QRadar SIEM Hub** | [https://www.ibm.com/docs/en/qsip/7.5](https://www.ibm.com/docs/en/qsip/7.5) | Official Product Documentation across all modules. |
| **IBM Security Learning Academy** | [https://www.securitylearningacademy.com/](https://www.securitylearningacademy.com/) | Hands-on labs, administration certifications & bootcamps. |
| **IBM Support Knowledge Center** | [https://www.ibm.com/support/home/](https://www.ibm.com/support/home/) | Technical notes, flash alerts, APAR bug reports, and hotfixes. |
| **IBM Redbooks Library** | [https://www.redbooks.ibm.com/](https://www.redbooks.ibm.com/) | Enterprise SIEM Deployment Guides, Tuning, and Best Practices. |

---

## 📑 Phase-by-Phase Official IBM Reference Directory

### Phase 1: SIEM Fundamentals & Deep QRadar Architecture
- 🔗 **QRadar Architecture & Data Flow**: [IBM Docs: Architecture Overview](https://www.ibm.com/docs/en/qsip/7.5?topic=overview-qradar-architecture)
- 🔗 **Component Process Tree & Daemons**: [IBM Docs: Core Components](https://www.ibm.com/docs/en/qsip/7.5?topic=overview-core-components)
- 🔗 **Events vs. Flows (QFlow & Network Activity)**: [IBM Docs: Network Activity & Flow Analytics](https://www.ibm.com/docs/en/qsip/7.5?topic=overview-network-activity)
- 🔗 **Common Port Matrix**: [IBM Docs: QRadar Common Ports](https://www.ibm.com/docs/en/qsip/7.5?topic=planning-common-ports)
- 🔗 **Token Bucket Licensing & Giveback Pool**: [IBM Docs: System & License Management](https://www.ibm.com/docs/en/qsip/7.5?topic=management-system-license-management)

### Phase 2: Enterprise Installation & Topology Setup
- 🔗 **Hardware & Virtual Appliance Specifications**: [IBM Docs: Appliance Specifications](https://www.ibm.com/docs/en/qsip/7.5?topic=planning-hardware-virtual-appliance-specifications)
- 🔗 **Installation Guide for QRadar**: [IBM Docs: QRadar Installation](https://www.ibm.com/docs/en/qsip/7.5?topic=installing-qradar-siem)
- 🔗 **Adding & Managing Managed Hosts**: [IBM Docs: Managing Managed Hosts](https://www.ibm.com/docs/en/qsip/7.5?topic=management-managing-managed-hosts)
- 🔗 **REST API Endpoints & Authorized Services**: [IBM Docs: REST API Guide](https://www.ibm.com/docs/en/qsip/7.5?topic=api-getting-started-qradar-rest-apis)
- 🔗 **Role-Based Access Control (RBAC) & Security Profiles**: [IBM Docs: User Management & Security Profiles](https://www.ibm.com/docs/en/qsip/7.5?topic=users-user-management)

### Phase 3: Deep Log Source Onboarding & Integration
- 🔗 **Log Source Management Overview**: [IBM Docs: Log Source Management](https://www.ibm.com/docs/en/qsip/7.5?topic=sources-log-source-management)
- 🔗 **DSM Configuration Guide (Per-Vendor Integration)**: [IBM Docs: DSM Configuration Guide](https://www.ibm.com/docs/en/qsip/7.5?topic=guide-dsm-configuration)
- 🔗 **WinCollect 7 & 10 Architecture Guide**: [IBM Docs: WinCollect Overview](https://www.ibm.com/docs/en/qsip/7.5?topic=wincollect-overview)
- 🔗 **Universal Cloud REST API Protocol**: [IBM Docs: Universal Cloud REST API Protocol Configuration](https://www.ibm.com/docs/en/qsip/7.5?topic=sources-universal-cloud-rest-api-protocol)
- 🔗 **JDBC Protocol Integration & Polling**: [IBM Docs: JDBC Protocol Configuration](https://www.ibm.com/docs/en/qsip/7.5?topic=sources-jdbc-protocol)

### Phase 4: DSM Parsing, Normalization & Custom Properties
- 🔗 **DSM Editor User Guide**: [IBM Docs: DSM Editor Overview](https://www.ibm.com/docs/en/qsip/7.5?topic=editor-dsm-overview)
- 🔗 **Event Normalization & QID Map Resolution**: [IBM Docs: Event Mapping](https://www.ibm.com/docs/en/qsip/7.5?topic=mapping-event)
- 🔗 **Custom Event Properties & Regular Expressions**: [IBM Docs: Custom Event and Flow Properties](https://www.ibm.com/docs/en/qsip/7.5?topic=properties-custom-event-flow)
- 🔗 **Log Source Extensions (LSX) XML Specification**: [IBM Docs: Log Source Extensions](https://www.ibm.com/docs/en/qsip/7.5?topic=extensions-log-source)

### Phase 5: Custom Rules Engine (CRE) & Offense Architecture
- 🔗 **Custom Rules Engine (CRE) User Guide**: [IBM Docs: Rules Overview](https://www.ibm.com/docs/en/qsip/7.5?topic=rules-custom-engine-overview)
- 🔗 **Rule Actions, Responses & Script Execution**: [IBM Docs: Rule Actions](https://www.ibm.com/docs/en/qsip/7.5?topic=rules-rule-actions)
- 🔗 **Building Blocks (BB) Architecture & Optimization**: [IBM Docs: Building Blocks](https://www.ibm.com/docs/en/qsip/7.5?topic=rules-building-blocks)
- 🔗 **Offense Lifecycle, Indexing & Magnitude Calculation**: [IBM Docs: Offense Management](https://www.ibm.com/docs/en/qsip/7.5?topic=offenses-investigating)
- 🔗 **Tuning Expensive Rules & Performance**: [IBM Support: Tuning Expensive Rules](https://www.ibm.com/support/pages/qradar-how-tune-expensive-custom-rules)

### Phase 6: Reference Data & Advanced Building Blocks
- 🔗 **Reference Data Management Guide**: [IBM Docs: Reference Data Collections](https://www.ibm.com/docs/en/qsip/7.5?topic=data-reference-overview)
- 🔗 **Threat Intelligence App & STIX/TAXII Ingestion**: [IBM Docs: Threat Intelligence App](https://www.ibm.com/docs/en/qsip/7.5?topic=apps-threat-intelligence)
- 🔗 **Reference Data REST API Endpoints**: [IBM Docs: Reference Data API](https://www.ibm.com/docs/en/qsip/7.5?topic=api-reference-data-endpoints)

### Phase 7: AQL Mastery & Ariel Storage Optimization
- 🔗 **AQL (Ariel Query Language) Guide**: [IBM Docs: AQL Overview](https://www.ibm.com/docs/en/qsip/7.5?topic=aql-overview)
- 🔗 **AQL Functions Reference Library**: [IBM Docs: AQL Functions](https://www.ibm.com/docs/en/qsip/7.5?topic=functions-aql-functions)
- 🔗 **Ariel Database Architecture & Storage**: [IBM Docs: Ariel Database Overview](https://www.ibm.com/docs/en/qsip/7.5?topic=ariel-database-overview)
- 🔗 **Custom Property Indexing for AQL Acceleration**: [IBM Docs: Index Management](https://www.ibm.com/docs/en/qsip/7.5?topic=management-index)

### Phase 8: Daily Administration, Linux CLI & Health Management
- 🔗 **System Notifications & Error Code Directory**: [IBM Support: System & License Notifications](https://www.ibm.com/support/pages/qradar-troubleshooting-system-and-license-notifications)
- 🔗 **QRadar Linux Command Line Utilities**: [IBM Support: Useful CLI Utilities](https://www.ibm.com/support/pages/qradar-useful-command-line-utilities)
- 🔗 **Disk Usage & `/store` Partition Management**: [IBM Support: Investigating /store Directory Usage](https://www.ibm.com/support/pages/qradar-disk-usage-information-and-investigating-store-directory)
- 🔗 **Java Heap Memory & JVM Tuning (`nva.conf`)**: [IBM Support: QRadar Memory Management & Tuning](https://www.ibm.com/support/pages/qradar-managing-and-troubleshooting-memory)

### Phase 9: High Availability (HA), Backup & Disaster Recovery
- 🔗 **High Availability Guide (DRBD & Corosync)**: [IBM Docs: High Availability Overview](https://www.ibm.com/docs/en/qsip/7.5?topic=cluster-high-availability-overview)
- 🔗 **HA Switchover & Failover Procedures**: [IBM Docs: HA Failover and Switchover](https://www.ibm.com/docs/en/qsip/7.5?topic=cluster-ha-failover-switchover)
- 🔗 **Troubleshooting DRBD Split-Brain Conditions**: [IBM Support: How to Recover from HA Split-Brain](https://www.ibm.com/support/pages/qradar-how-recover-ha-split-brain)
- 🔗 **Backup & Disaster Recovery Procedures**: [IBM Docs: Backup and Restore](https://www.ibm.com/docs/en/qsip/7.5?topic=recovery-backup-restore)
- 🔗 **Data Node (14xx) Architecture & Storage Expansion**: [IBM Docs: Data Nodes Overview](https://www.ibm.com/docs/en/qsip/7.5?topic=nodes-data-overview)

### Phase 10: Advanced Troubleshooting & Master Mock Interviews
- 🔗 **Troubleshooting Ingestion & Dropped Events**: [IBM Support: Dropped Events Due to License](https://www.ibm.com/support/pages/qradar-troubleshooting-dropped-events-due-license)
- 🔗 **WinCollect Troubleshooting & TLS Handshakes**: [IBM Support: WinCollect Troubleshooting Guide](https://www.ibm.com/support/pages/wincollect-troubleshooting-communication-and-certificates)
- 🔗 **ActiveMQ / IMQ Message Queue Troubleshooting**: [IBM Support: ActiveMQ Queue Issues in QRadar](https://www.ibm.com/support/pages/qradar-troubleshooting-activemq-message-broker)
- 🔗 **QRadar SSL Certificate Replacement Procedure**: [IBM Support: Installing SSL Certificates with install-ssl-cert.sh](https://www.ibm.com/support/pages/qradar-how-install-new-ssl-certificate)

---

## 🔒 Commitment to Quality & Verification

Every configuration stanza, CLI binary path, log file location, protocol port, and architectural diagram across this 90-day masterclass has been verified against these official IBM engineering publications to ensure 100% production fidelity.
