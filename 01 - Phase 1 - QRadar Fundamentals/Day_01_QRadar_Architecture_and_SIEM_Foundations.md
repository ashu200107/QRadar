# 📅 Day 01: QRadar Architecture & SIEM Foundations

> **Phase 1: SIEM Fundamentals & Deep QRadar Architecture**  
> **Topic:** SIEM Mechanics, Core QRadar Anatomy, Daemons, 8-Stage Pipeline, Events vs Flows  
> **Estimated Study Time:** 2 Hours + Hands-on Lab & Quiz Submission

---

## 📸 Day 1 Visual Architecture Gallery

| 1. Enterprise Distributed Topology | 2. 8-Stage Ingestion Pipeline |
| :---: | :---: |
| ![Enterprise 3D Topology\|500](../assets/01_enterprise_architecture_3d.jpg) | ![8-Stage Ingestion Pipeline\|500](../assets/02_data_pipeline_8stages_3d.jpg) |
| *Figure 1: Full QRadar Distributed Topology (`![[01_enterprise_architecture_3d.jpg|500]]`)* | *Figure 2: Pipeline from Ingestion to Ariel & Offenses (`![[02_data_pipeline_8stages_3d.jpg|500]]`)* |

| 3. Ariel Columnar Time-Series Storage | 4. Events vs. Network Flows (QFlow DPI) |
| :---: | :---: |
| ![Ariel Storage 3D\|500](../assets/03_ariel_storage_columnar_3d.jpg) | ![Events vs Flows 3D\|500](../assets/04_events_vs_flows_qflow_3d.jpg) |
| *Figure 3: Columnar Payloads vs Records (`![[03_ariel_storage_columnar_3d.jpg|500]]`)* | *Figure 4: Discrete EPS vs Continuous QFlow Telemetry (`![[04_events_vs_flows_qflow_3d.jpg|500]]`)* |

---

## 🎯 Day 1 Learning Objectives
By the end of this lesson, you will be able to:
1. Explain the architectural and operational differences between raw Syslog log aggregation and QRadar SIEM.
2. Identify and describe every core QRadar appliance role (Console 31xx, EC 15xx, EP 16xx, FC/FP 17xx, Data Node 14xx, App Host).
3. Map every critical QRadar Linux daemon (`hostcontext`, `ecs-ec-ingress`, `ecs-ep`, `tomcat`, `postgres`, `imq`, `magistrate`) to its operational responsibility.
4. Trace an incoming log packet through the 8-stage pipeline from memory queue to Ariel disk storage and Offense generation.
5. Differentiate Events from Network Flows and explain Deep Packet Inspection (QFlow).
6. Execute essential Linux CLI commands to verify QRadar service states, listening ports, and storage partitions.

---

## 📚 Core Study Materials
- 📄 [Chapter 01: What is SIEM & IBM QRadar Overview](./Chapter_01_What_is_SIEM_and_QRadar_Overview.md)
- 📄 [Chapter 02: QRadar Core Architecture & Component Breakdown](./Chapter_02_QRadar_Core_Architecture_and_Components.md)
- 📄 [Chapter 03: The End-to-End 8-Stage Event Pipeline](./Chapter_03_The_End_to_End_Event_Pipeline.md)
- 📄 [Chapter 04: Events vs. Network Flows Deep Dive](./Chapter_04_Events_vs_Flows_Deep_Dive.md)
- 🧪 [Lab 01: Architecture & Pipeline Mapping via CLI](../Labs/Lab_01_Architecture_and_Pipeline_Mapping.md)

---

## 🛠️ Linux CLI Cheat Sheet for Day 1

```bash
# 1. Inspect QRadar storage partition utilization
df -h /store /store/ariel /store/transient

# 2. Check the master supervisor process
systemctl status hostcontext

# 3. Check Event Collection and Event Processing daemons
ps aux | grep -E "ecs-ec-ingress|ecs-ep" | grep -v grep

# 4. Check active listening ports (514 Syslog, 443 UI, 32006 Inter-Appliance)
ss -tulpn | grep -E "514|443|32006"

# 5. Tail live QRadar logs for errors and pipeline warnings
tail -n 25 /var/log/qradar.log
tail -n 25 /var/log/qradar.error
```

---

## 📝 Day 1 Quiz & Live Interview Assessment

Submit your answers to these 16 questions to evaluate your baseline competency.

### Section A: 5 Basic Questions
1. What is the fundamental difference between an **Event** and a **Network Flow** in QRadar?
2. Which specific QRadar process is responsible for event collection and buffering, and which process handles rule evaluation (CRE)?
3. What is the exact path on disk where raw and normalized historical event data is stored?
4. Which network port is used for inter-appliance log streaming between an Event Collector and an Event Processor?
5. What role does PostgreSQL play in a QRadar deployment compared to the Ariel database?

### Section B: 5 Intermediate Questions
6. Explain the **Token Bucket** concept in QRadar and what occurs at the `ecs-ec-ingress` layer when an appliance exceeds its licensed EPS.
7. What is a **QID** in QRadar, and at which stage of the 8-stage pipeline is an event assigned a QID?
8. What is the functional role of the **Magistrate** component, and on which appliance does it reside?
9. Why does QRadar use a separate `App Host` instead of running all applications directly on the Console?
10. If an Event Processor's disk space hits 95% full on `/store`, what automated protective action does QRadar take regarding log ingestion and the pipeline?

### Section C: 3 Real-World Scenario Questions
11. **Scenario 1**: A network engineer points 2,000 EPS of firewall syslog traffic to an Event Collector licensed for 1,000 EPS. What happens to the excess logs in real time, and what alerts will appear in the Console?
12. **Scenario 2**: An analyst reports that they can run searches for yesterday's logs, but any search covering logs from 30 days ago immediately fails with a query timeout. As an administrator, what component and storage area do you suspect?
13. **Scenario 3**: You log in via SSH to an All-in-One Console and find that `df -h /store` shows 98% utilization. The Web UI is sluggish and offenses have stopped generating. What is your immediate diagnostic step?

### Section D: 2 Deep-Dive Troubleshooting Questions
14. An administrator notices that `ecs-ec-ingress` is running, but port `514 UDP` is not accepting packets. What Linux and QRadar checks would you execute to isolate whether the issue is OS-level, firewall-level, or application-level?
15. What log file would you inspect to troubleshoot why the `hostcontext` service is failing to start or communicate with managed hosts?

### Section E: 1 Real-World Interview Question (Target: 2-Year Experienced SIEM Engineer)
16. **Interviewer Prompt**:
    > *"Walk me through the complete end-to-end event flow in IBM QRadar. Start from the moment a firewall generates a syslog packet to the point where an Offense is displayed on the SOC analyst's dashboard. Mention the components, daemons, and storage layers involved."*
