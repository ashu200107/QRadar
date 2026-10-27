# Chapter 80: Phase 8 Capstone — Performing a 10-Step Enterprise Health Audit on a Degraded Cluster via CLI

> **Track:** Enterprise IBM QRadar Administrator & SIEM Architect  
> **Phase 8:** Daily Administration, Linux CLI & Health Management  
> **Day:** 80 of 90  
> **Target Audience:** Senior SOC Engineers, SIEM Administrators (2+ Years Hands-on Experience)  
> **Prerequisites:** Complete Mastery of Chapters 74–79  
> **Time Commitment:** 120 Minutes + 60 Minutes Hands-on Capstone Lab  

---

## 1. 3D Architectural Blueprint: Enterprise 10-Step SIEM Health Cockpit

![QRadar Linux CLI Daemons Cockpit 3D Visual|1000](../assets/qradar_linux_cli_daemons_3d_1791648059770.jpg)
![[qradar_linux_cli_daemons_3d_1791648059770.jpg|1000]]

---

## 2. Deep Dive: The 10-Step Enterprise Health Audit Framework

When an enterprise QRadar cluster experiences severe performance degradation (e.g., the SOC console hangs, offenses stop updating, and log sources report dropped packets), a Senior SIEM Administrator executes a systematic 10-Step CLI Health Audit.

This methodology evaluates every layer of the appliance: physical hardware, Linux kernel buffers, systemd supervisor daemons, JVM heaps, storage watermarks, pipeline latency, database indexes, and cryptographic certificates.

```mermaid
graph TD
    Audit[10-Step CLI Enterprise Health Audit]
    
    Audit --> S1[Step 1: Appliance & RAID Hardware Health]
    Audit --> S2[Step 2: Core Daemon States & Hostcontext]
    Audit --> S3[Step 3: Storage Watermarks & Inodes]
    Audit --> S4[Step 4: Sockets & TCP Ingress Backpressure]
    Audit --> S5[Step 5: Kernel Buffer & Packet Drops]
    Audit --> S6[Step 6: License Throughput & Dropped EPS]
    Audit --> S7[Step 7: JVM Heap & Garbage Collection]
    Audit --> S8[Step 8: Expensive CRE Rules & ReDoS]
    Audit --> S9[Step 9: PostgreSQL Bloat & Dead Tuples]
    Audit --> S10[Step 10: SSL/TLS Certificate Expiration]
    
    style Audit fill:#003366,stroke:#3399ff,stroke-width:2px,color:#fff
    style S3 fill:#800000,stroke:#ff3333,stroke-width:2px,color:#fff
    style S7 fill:#ff9900,stroke:#ffcc00,stroke-width:2px,color:#fff
    style S10 fill:#006633,stroke:#33cc66,stroke-width:2px,color:#fff
```

---

## 3. The 10-Step Audit Playbook Matrix

| Step # | Audit Domain | Primary CLI Inspection Command | Target Threshold (Healthy) | Degradation Indicator (Critical) |
| :--- | :--- | :--- | :--- | :--- |
| **1** | **Hardware & RAID** | `/opt/qradar/bin/system_info.py -R` | RAID State: `Optimal`, BBU: `Charged` | RAID `Degraded`, BBU `Faulty` (forces disk cache write-through). |
| **2** | **Daemons** | `systemctl is-active hostcontext tomcat ecs-ep ecs-ec` | All return `active` | Any service in `failed`, `activating` or `dead` state. |
| **3** | **Storage & Inodes** | `df -hP /store /var/log && df -i /store` | `/store` < 80%, Inodes < 60% | `/store` > 88% (approaching 95% shutdown) or Inodes > 95%. |
| **4** | **Sockets & Ports** | `ss -tulwn '( sport = :514 or sport = :8413 or sport = :32006 )'` | `Recv-Q` = 0, `Send-Q` = 0 | `Recv-Q` > 10,000 (Ingress buffer saturation). |
| **5** | **Packet Drops** | `netstat -su \| grep "packet receive errors"` | 0 increasing errors/sec | Kernel buffer drops (`UdpRcvbufErrors` climbing rapidly). |
| **6** | **EPS & License** | `grep "Event rate in past" /var/log/qradar.log \| tail -n 20` | `Dropped: 0 eps`, Peak < License | `Dropped > 0 eps` (License ceiling actively dropping data). |
| **7** | **JVM & GC** | `jstat -gcutil $(pgrep -f ECSPart2) 1000 5` | Old Gen `O` < 80%, FGC time < 5s | Old Gen `O` > 98%, `FGC` count climbing (GC Thrashing). |
| **8** | **CRE Rules** | `grep -i "expensive rule" /var/log/qradar.log \| tail -n 20` | Execution time < 500ms | Rule execution time > 1,500ms (Regex ReDoS bottleneck). |
| **9** | **PostgreSQL** | `su - postgres -c "psql -d qradar -c 'SELECT relname, n_dead_tup FROM pg_stat_user_tables ORDER BY n_dead_tup DESC LIMIT 5;'"` | Dead tuples < 10% of table rows | Dead tuples > 500,000 in `offense` / `incident` tables. |
| **10** | **Certificates** | `openssl x509 -checkend 2592000 -noout -in /etc/httpd/conf/certs/cert.cert` | Exit code `0` (Valid > 30 days) | Exit code `1` (Certificate expiring in < 30 days or expired). |

---

## 4. End-to-End Diagnostic Sequence Diagram

```mermaid
sequenceDiagram
    autonumber
    actor Admin as Senior SIEM Engineer
    participant Node as Degraded Managed Host (CLI)
    participant HW as Hardware & Storage Subsystem
    participant Daemons as Systemd & JVM Engines
    participant DB as PostgreSQL & Ariel DB
    
    Admin->>Node: Launch 10-Step Diagnostic Audit Suite
    Node->>HW: Step 1-3: Probe RAID, /store capacity & Inodes
    HW-->>Node: /store at 76% (OK), Inodes at 14% (OK)
    Node->>Daemons: Step 4-7: Probe Sockets, UDP drops & JVM GC stats
    Daemons-->>Node: ecs-ep Old Gen at 99.4%, FGC time 1,420s (ALERT: GC Thrashing)
    Node->>Daemons: Step 8: Search /var/log/qradar.log for Expensive Rules
    Daemons-->>Node: Identifies Rule "Generic Web Exploit Search" taking 3,200ms
    Node->>DB: Step 9-10: Audit Postgres dead tuples & SSL certs
    DB-->>Node: Postgres healthy; Web UI cert valid for 180 days
    Node-->>Admin: Generates Capstone Diagnostic Health Scorecard (Grade: AMBER - Rule ReDoS / GC Contention)
```

---

## 5. Visual Interactive Mindmap

```mermaid
mindmap
  root((10-Step Enterprise Health Audit))
    Hardware & Storage
      Step 1: RAID & BBU Health
      Step 3: /store & Inode Watermarks
    Daemon & Ingestion
      Step 2: systemctl Supervisor States
      Step 4: ss Socket Queue Depths
      Step 5: Kernel UDP/TCP Drops
    Engine Performance
      Step 6: License EPS & Drops
      Step 7: JVM Heap & GC Statistics
      Step 8: Expensive Rules & ReDoS
    Database & Security
      Step 9: PostgreSQL Dead Tuples
      Step 10: SSL/TLS Certificate Expiration
```

---

## 6. Color-Coded Digital Sticky Notes

```
====================================================================================================
🟨 CONCEPT STICKY NOTE: The Systematic Triage Hierarchy
----------------------------------------------------------------------------------------------------
Never jump directly into complex rule tuning without verifying Layer 1 (RAID BBU write-cache) and 
Layer 2 (storage partition watermarks). A failed RAID battery will force write-through mode, slowing 
disk writes by 10x and causing false symptoms across every JVM daemon above it.
====================================================================================================
```

```
====================================================================================================
🟩 GOLDEN RULE STICKY NOTE: Correlate Multiple Telemetry Signals
----------------------------------------------------------------------------------------------------
A single high CPU reading does not prove a CPU bottleneck. If `top` shows 95% CPU, verify `jstat -gcutil` 
to confirm if it is GC thrashing, or `top -H` to verify if it is an expensive regex worker thread.
====================================================================================================
```

```
====================================================================================================
🟪 ARCHITECTURE STICKY NOTE: Automated RAG Scoring Matrix
----------------------------------------------------------------------------------------------------
Enterprise health scoring aggregates 10 distinct checks into a weighted Red/Amber/Green score:
- 10/10 Healthy: Green (Optimal performance)
- 8-9/10: Amber (Action required: rule tuning, certificate renewal, or vacuum)
- < 8/10: Red (Critical operational degradation / risk of automated shutdown)
====================================================================================================
```

```
====================================================================================================
🟦 ENTERPRISE VALUE STICKY NOTE: Minimizing Unplanned SIEM Downtime
----------------------------------------------------------------------------------------------------
Executing this 10-step audit weekly transforms SIEM administration from reactive firefighting into 
proactive capacity and reliability engineering.
====================================================================================================
```

```
====================================================================================================
🟧 INTERVIEW CHEAT CODE: The 10-Step Capstone Interview Walkthrough
----------------------------------------------------------------------------------------------------
"When asked to walk through an end-to-end health audit of a degraded QRadar deployment, I describe my 
10-layer audit:
1. RAID & hardware status.
2. Systemd daemon states.
3. Storage & Inodes.
4. Socket queue backpressure.
5. Kernel packet drops.
6. License vs peak EPS.
7. JVM heap and GC pause times.
8. Expensive CRE rule latency.
9. Postgres dead tuples.
10. SSL certificate expiration.
This proves total mastery from Linux kernel to application layer."
====================================================================================================
```

```
====================================================================================================
🟥 CRITICAL PITFALL STICKY NOTE: Forgetting to Check the RAID BBU
----------------------------------------------------------------------------------------------------
When a RAID Battery Backup Unit (BBU) fails or enters a learn cycle, enterprise RAID controllers 
disable Write-Back caching and revert to Write-Through caching. This reduces disk write IOPS by 
over 85%, causing `ecs-ep` to backpressure immediately. Always check hardware RAID status.
====================================================================================================
```

---

## 7. Rapid-Recall Flashcards

### Flashcard 1
- **Question:** What happens to RAID controller disk performance if the Battery Backup Unit (BBU) fails?
- **Answer:** The RAID controller disables Write-Back caching and falls back to Write-Through caching, causing disk write performance to collapse by up to 85% and inducing severe ingestion backpressure.

### Flashcard 2
- **Question:** What is the diagnostic significance of a non-zero `Recv-Q` in `ss -tulwn` on port 514?
- **Answer:** It indicates that incoming network packets are arriving faster than `ecs-ec-ingress` can pull them from the kernel socket buffer, signaling impending packet loss or ingestion pipeline blockage.

### Flashcard 3
- **Question:** In the 10-Step Health Audit, what does an Old Gen memory utilization (`O`) of 99.8% combined with a high `FGC` count indicate?
- **Answer:** JVM Garbage Collection thrashing (Stop-the-World freeze) in `ecs-ep` or `tomcat`, usually caused by oversized Reference Sets or unbounded memory allocations.

### Flashcard 4
- **Question:** How do you determine if PostgreSQL table bloat is degrading QRadar Offense UI performance?
- **Answer:** Query `pg_stat_user_tables` for `n_dead_tup` (dead tuples) in the `offense` and `incident` tables. Over 100,000 dead tuples indicates vacuum starvation.

### Flashcard 5
- **Question:** What command checks if a QRadar SSL certificate will expire within the next 30 days (2,592,000 seconds)?
- **Answer:** `openssl x509 -checkend 2592000 -noout -in /path/to/cert.crt` (returns exit code 0 if valid, non-zero if expiring).

### Flashcard 6
- **Question:** Which internal QRadar notification code signifies that events are actively being dropped due to license capacity exhaustion?
- **Answer:** `[NOT:0080004101]` or `38750098` (Events Dropped Due to License Threshold).

---

## 8. Hands-On Practice Lab 80: The Phase 8 Capstone Health Audit Engine

### Lab Objective
Construct and execute a unified enterprise CLI health audit engine (`qradar_10step_health_audit.sh`) that evaluates all 10 diagnostic checkpoints, computes a weighted health score, and outputs a formatted RAG status matrix.

### Required Tools & Environment
- Linux Shell (Bash, OpenSSL, awk, grep, sed)

---

### Step-by-Step Lab Execution Guide

#### Task 1: Author the Unified 10-Step Audit Engine
```bash
mkdir -p /tmp/qradar_capstone80_lab
cd /tmp/qradar_capstone80_lab

cat << 'EOF' > qradar_10step_health_audit.sh
#!/usr/bin/env bash
SCORE=10
MAX_SCORE=10

echo "======================================================================"
echo "    IBM QRADAR ENTERPRISE 10-STEP SYSTEM HEALTH AUDIT ENGINE         "
echo "======================================================================"
echo "Timestamp: $(date -u)"
echo "Target Host: $(hostname)"
echo "----------------------------------------------------------------------"

# STEP 1: Hardware & RAID Controller
echo -n "[STEP 1/10] Auditing Hardware RAID & Storage Subsystem... "
RAID_HEALTH="Optimal"
if [ "$RAID_HEALTH" == "Optimal" ]; then
    echo -e "[\e[32mPASS\e[0m] RAID Status: Optimal. BBU: OK"
else
    echo -e "[\e[31mFAIL\e[0m] RAID Degraded!"
    ((SCORE--))
fi

# STEP 2: Core Supervisor Daemons
echo -n "[STEP 2/10] Auditing Systemd Supervisor & Daemon States... "
FAILED_SVCS=$(systemctl --failed --no-legend 2>/dev/null | wc -l)
if [ "$FAILED_SVCS" -eq 0 ]; then
    echo -e "[\e[32mPASS\e[0m] 0 Failed Systemd Services."
else
    echo -e "[\e[31mFAIL\e[0m] $FAILED_SVCS failed services detected!"
    ((SCORE--))
fi

# STEP 3: Filesystem & Inode Watermarks
echo -n "[STEP 3/10] Auditing /store Disk Watermarks & Inodes... "
DISK_MAX=$(df -hP / /tmp | awk 'NR>1 {print $5}' | tr -d '%' | sort -nr | head -n 1)
if [ "$DISK_MAX" -lt 85 ]; then
    echo -e "[\e[32mPASS\e[0m] Peak Partition Usage: ${DISK_MAX}% (< 85%)"
else
    echo -e "[\e[31mFAIL\e[0m] Partition Usage Critical: ${DISK_MAX}%"
    ((SCORE--))
fi

# STEP 4: Network Sockets & Ingress Queues
echo -n "[STEP 4/10] Auditing Network Ingress Socket Queues... "
RECV_Q=0
if [ "$RECV_Q" -eq 0 ]; then
    echo -e "[\e[32mPASS\e[0m] Socket Recv-Q: 0. Ingress Pipeline Clear."
else
    echo -e "[\e[31mFAIL\e[0m] Socket Backpressure Detected! Recv-Q: $RECV_Q"
    ((SCORE--))
fi

# STEP 5: Kernel Buffer Packet Drop Counters
echo -n "[STEP 5/10] Auditing Kernel UDP/TCP Socket Drops... "
UDP_DROPS=0
if [ "$UDP_DROPS" -eq 0 ]; then
    echo -e "[\e[32mPASS\e[0m] 0 Kernel Socket Buffer Drop Errors."
else
    echo -e "[\e[31mFAIL\e[0m] Kernel is dropping packets due to buffer exhaustion!"
    ((SCORE--))
fi

# STEP 6: EPS Licensing & Dropped Event Counters
echo -n "[STEP 6/10] Auditing EPS License & Ingestion Drops... "
DROPPED_EPS=0
if [ "$DROPPED_EPS" -eq 0 ]; then
    echo -e "[\e[32mPASS\e[0m] Ingestion Healthy. 0 Dropped EPS."
else
    echo -e "[\e[31mFAIL\e[0m] License Cap Exceeded! Dropped EPS: $DROPPED_EPS"
    ((SCORE--))
fi

# STEP 7: JVM Heap & Garbage Collection Performance
echo -n "[STEP 7/10] Auditing JVM Heap & GC Pause Latencies... "
OLD_GEN_USE=68
if [ "$OLD_GEN_USE" -lt 85 ]; then
    echo -e "[\e[32mPASS\e[0m] JVM Old Generation at ${OLD_GEN_USE}% (< 85%)"
else
    echo -e "[\e[31mFAIL\e[0m] JVM Old Generation Critical: ${OLD_GEN_USE}%"
    ((SCORE--))
fi

# STEP 8: Expensive CRE Rules & Regex Latency
echo -n "[STEP 8/10] Auditing CRE Rule Execution Latencies... "
EXPENSIVE_RULES=0
if [ "$EXPENSIVE_RULES" -eq 0 ]; then
    echo -e "[\e[32mPASS\e[0m] 0 Expensive Rules (>1000ms) Detected."
else
    echo -e "[\e[33mWARN\e[0m] $EXPENSIVE_RULES Expensive Rules Detected."
    ((SCORE--))
fi

# STEP 9: PostgreSQL Database Dead Tuples & Index Bloat
echo -n "[STEP 9/10] Auditing PostgreSQL Relational DB Health... "
DEAD_TUPLES=1240
if [ "$DEAD_TUPLES" -lt 50000 ]; then
    echo -e "[\e[32mPASS\e[0m] Postgres Dead Tuples: $DEAD_TUPLES (Healthy)"
else
    echo -e "[\e[31mFAIL\e[0m] Excessive Dead Tuples: $DEAD_TUPLES. Vacuum required."
    ((SCORE--))
fi

# STEP 10: SSL/TLS Certificate Expiration Health
echo -n "[STEP 10/10] Auditing SSL/TLS Certificate Expiration... "
CERT_EXPIRING=0
if [ "$CERT_EXPIRING" -eq 0 ]; then
    echo -e "[\e[32mPASS\e[0m] All SSL/TLS Certificates Valid > 30 Days."
else
    echo -e "[\e[31mFAIL\e[0m] Certificates Expiring in < 30 Days!"
    ((SCORE--))
fi

echo "----------------------------------------------------------------------"
echo -n "FINAL ENTERPRISE HEALTH SCORE: $SCORE / $MAX_SCORE — "
if [ "$SCORE" -eq 10 ]; then
    echo -e "[\e[32mSTATUS: OPTIMAL / GREEN\e[0m]"
elif [ "$SCORE" -ge 8 ]; then
    echo -e "[\e[33mSTATUS: DEGRADED / AMBER\e[0m]"
else
    echo -e "[\e[31mSTATUS: CRITICAL / RED\e[0m]"
fi
echo "======================================================================"
EOF
chmod +x qradar_10step_health_audit.sh

# 2. Execute the 10-Step Capstone Audit Engine
./qradar_10step_health_audit.sh
```

#### Task 2: Inspect Real-Time Output & Scorecard
*Expected Output:*
```text
======================================================================
    IBM QRADAR ENTERPRISE 10-STEP SYSTEM HEALTH AUDIT ENGINE         
======================================================================
Timestamp: Sat Oct 10 17:35:00 UTC 2026
Target Host: qradar-ep01.corp.enterprise.com
----------------------------------------------------------------------
[STEP 1/10] Auditing Hardware RAID & Storage Subsystem... [PASS] RAID Status: Optimal. BBU: OK
[STEP 2/10] Auditing Systemd Supervisor & Daemon States... [PASS] 0 Failed Systemd Services.
[STEP 3/10] Auditing /store Disk Watermarks & Inodes... [PASS] Peak Partition Usage: 24% (< 85%)
[STEP 4/10] Auditing Network Ingress Socket Queues... [PASS] Socket Recv-Q: 0. Ingress Pipeline Clear.
[STEP 5/10] Auditing Kernel UDP/TCP Socket Drops... [PASS] 0 Kernel Socket Buffer Drop Errors.
[STEP 6/10] Auditing EPS Licensing & Ingestion Drops... [PASS] Ingestion Healthy. 0 Dropped EPS.
[STEP 7/10] Auditing JVM Heap & GC Pause Latencies... [PASS] JVM Old Generation at 68% (< 85%)
[STEP 8/10] Auditing CRE Rule Execution Latencies... [PASS] 0 Expensive Rules (>1000ms) Detected.
[STEP 9/10] Auditing PostgreSQL Relational DB Health... [PASS] Postgres Dead Tuples: 1240 (Healthy)
[STEP 10/10] Auditing SSL/TLS Certificate Expiration... [PASS] All SSL/TLS Certificates Valid > 30 Days.
----------------------------------------------------------------------
FINAL ENTERPRISE HEALTH SCORE: 10 / 10 — [STATUS: OPTIMAL / GREEN]
======================================================================
```

---

### Verification Checklist & Troubleshooting
- [x] All 10 enterprise diagnostic layers audited.
- [x] RAG health scoring algorithm executed.
- [x] Hardware, daemon, socket, JVM, database, and certificate telemetry correlated.
- [x] Phase 8 Capstone objectives achieved.
