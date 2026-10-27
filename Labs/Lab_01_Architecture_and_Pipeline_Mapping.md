# 🧪 Lab 01: Architecture & Pipeline Mapping via CLI

> **Phase 1: QRadar Fundamentals** | Lab Difficulty: Beginner to Intermediate  
> **Estimated Duration:** 45 Minutes  
> **Prerequisites:** Access to a Linux terminal or QRadar SSH session

---

## 🎯 Lab Objectives
1. Verify the runtime health of core QRadar supervisor and event processing daemons via Linux CLI.
2. Audit network socket bindings and confirm listening ports for collection and inter-appliance transport.
3. Inspect storage partitions (`/store`, `/store/ariel`, `/store/transient`) and verify disk spooling directories.
4. Trace live log flow and identify system warning patterns in `/var/log/qradar.log`.

---

## 🛠️ Step-by-Step Lab Execution

### Step 1: Storage Partition & Disk Spool Verification
Inspect the available disk space on the primary QRadar partitions.

```bash
# Check filesystem utilization
df -h /store /store/ariel /store/transient

# Check transient spooling folder status
ls -ld /store/transient /store/spool 2>/dev/null
```
> **Expected Output:**
> - `/store` partition should be under 80% utilization.
> - `/store/transient` should exist with permissions owned by `nobody` or `qradar`.

---

### Step 2: Master Daemon State Verification
Check the status of `hostcontext` (the supervisor daemon managing all managed hosts and processes).

```bash
# Check hostcontext systemd service
systemctl status hostcontext
```
> **Expected Output:**
> - Service state should be `active (running)`.

---

### Step 3: Event Collection and Event Processing Process Inspection
Confirm that `ecs-ec-ingress` and `ecs-ep` are actively executing and inspect their Java memory allocation.

```bash
# Check running processes
ps aux | grep -E "ecs-ec-ingress|ecs-ep" | grep -v grep
```
> **Expected Output:**
> - `ecs-ec-ingress` (Event Collector ingestion process).
> - `ecs-ep` (Event Processing & CRE engine).

---

### Step 4: Network Socket & Port Binding Audit
Verify that QRadar is actively listening on inbound collection ports and inter-appliance streaming ports.

```bash
# Using ss (Socket Statistics)
ss -tulpn | grep -E "514|443|22|32006|8413"

# Using netstat alternative
netstat -tulpen | grep -E "514|443|22|32006|8413"
```
> **Port Matrix Verification:**
> - `*:514` (UDP/TCP) $\rightarrow$ `ecs-ec-ingress` (Syslog listener)
> - `*:443` (TCP) $\rightarrow$ `httpd` (Web UI reverse proxy)
> - `*:32006` (TCP) $\rightarrow$ `ecs-ep` (Inter-appliance Event Stream listener)
> - `*:8413` (TCP) $\rightarrow$ `ecs-ec-ingress` (WinCollect agent port)

---

### Step 5: Live Pipeline Log Tracing
Tail the active QRadar system log to observe background system health checks and license tracking.

```bash
# Tail live operations log
tail -f /var/log/qradar.log | grep -E "License|Pipeline|Ariel|CRE"
```

---

## 📋 Lab Verification Questions
1. If `/store/transient` is accumulating hundreds of `.spool` files that are not decreasing, what does this indicate about downstream communication?
2. If `ss -tulpn | grep :514` returns empty, which specific QRadar process needs to be investigated and restarted?
