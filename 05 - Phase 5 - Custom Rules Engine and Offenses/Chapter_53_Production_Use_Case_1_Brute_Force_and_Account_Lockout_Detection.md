# 📖 Chapter 53: Production Use Case 1: Brute Force & Account Lockout Detection Masterclass

> **The IBM QRadar Administrator's Technical Guide** | Phase 5: Custom Rules Engine (CRE) & Offense Architecture  
> **Target Audience:** 1.5-Year Administrators $\rightarrow$ Senior SIEM Engineers & Detection Engineering Specialists  
> **Focus:** Multi-Source Authentication Telemetry, Vertical Brute Force vs Password Spraying, Windows 4625/4740, & Hands-on Lab 53

---

## 📸 1. Visual Overview: Threat Correlation — Brute Force & Credential Attacks

![Threat Correlation Use Cases|1000](../assets/23_threat_correlation_usecases_3d.jpg)
*Figure 53.1: 3D Multi-Stage Threat Correlation in QRadar: Stage 1 Ingress showing External IPs executing Brute Force / Credential Stuffing against User Authentication Servers and Active Directory Domain Controllers (`![[23_threat_correlation_usecases_3d.jpg|1000]]`)*

---

## 2. Threat Landscape: Vertical Brute Force vs Horizontal Password Spraying

Authentication attacks remain the #1 initial access vector in enterprise breaches. Detection engineers must distinguish between two fundamentally distinct attack modalities:

```
┌────────────────────────────────────────────────────────────────────────┐
│ 🔐 THE 2 AUTHENTICATION ATTACK PROFILES                                │
│                                                                        │
│ 1. VERTICAL BRUTE FORCE (Single User, Many Passwords)                  │
│    • Attack: 1 Source IP attempts 500 passwords against user `admin`.  │
│    • Outcome: High event rate, triggers Active Directory lockout.      │
│    • Detection Index: Index by `Source IP` or `Username`.              │
│ ├────────────────────────────────────────────────────────────────────┤
│ 2. HORIZONTAL PASSWORD SPRAYING (Many Users, Single Password)          │
│    • Attack: 1 Source IP tests `Winter2026!` against 5,000 usernames.  │
│    • Rate: 1 attempt per account every 30 minutes (evades lockouts).   │
│    • Detection Index: Index by `Source IP` (Count Unique Usernames).   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Telemetry Sources & Critical Event IDs

To build an enterprise detection pipeline, QRadar ingests authentication logs across multiple platforms:

| Telemetry Source | Success Event ID / QID | Failure Event ID / QID | Lockout Event ID / QID | Key Parsed Fields |
| :--- | :--- | :--- | :--- | :--- |
| **Windows Active Directory / WinCollect** | `4624` (Logon Success) | `4625` (Logon Failure) | `4740` (Account Locked Out) | `TargetUserName`, `IpAddress`, `WorkstationName`, `SubStatus` |
| **Linux / Unix SSH (`sshd`)** | `Accepted password/publickey` | `Failed password / invalid user` | `pam_tally2 / faillock` | `username`, `sourceip`, `sourceport` |
| **Microsoft Entra ID / Cloud IAM** | `50125` / `50140` | `50126` (Invalid Password) | `50053` (Account Locked) | `UserPrincipalName`, `IPAddress`, `ClientApp` |
| **Cisco AnyConnect / Fortinet VPN** | `713149` (Tunnel Established) | `713150` (Auth Failed) | `Account Disabled` | `Username`, `CallingStationID`, `SrcIP` |

---

## 4. Production Rule Architecture 1: Single-Source Vertical Brute Force

```
┌────────────────────────────────────────────────────────────────────────┐
│ 📋 RULE SPECIFICATION: UBA: Vertical Brute Force (Single Source IP)     │
│                                                                        │
│ 🔍 RULE TEST CONDITIONS:                                               │
│    • Apply "UBA: Vertical Brute Force" on events which are detected    │
│      by the Local system                                               │
│    • and when the event matches BB:CategoryDefinition: Authentication  │
│      Failures                                                          │
│    • and when at least 15 events occur with the same Source IP in      │
│      3 minutes                                                         │
│    • AND NOT when the event matches BB:FalsePositive: Authorized       │
│      Vulnerability Scanners and Pentest IPs                            │
│                                                                        │
│ ⚙️ RULE ACTIONS:                                                       │
│    • Ensure detected event is part of an OFFENSE                       │
│    • Index Offense by: Source IP                                       │
│    • Set Severity to: 6, Credibility to: 8, Relevance to: 7            │
│    • Annotate: "Multiple failed logins detected from single source IP" │
│                                                                        │
│ 📢 RULE RESPONSES:                                                     │
│    • Dispatch New Event: "Vertical Brute Force Detected" (QID 28250010)│
│    • Limit response: At most once every 15 minutes per Source IP       │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 5. Production Rule Architecture 2: Distributed Password Spraying

```
┌────────────────────────────────────────────────────────────────────────┐
│ 📋 RULE SPECIFICATION: UBA: Distributed Password Spraying Detection    │
│                                                                        │
│ 🔍 RULE TEST CONDITIONS:                                               │
│    • Apply "UBA: Distributed Password Spraying" on events              │
│    • and when the event matches BB:CategoryDefinition: Authentication  │
│      Failures                                                          │
│    • and when at least 20 events occur with the same Source IP and     │
│      DIFFERENT Username in 10 minutes                                  │
│    • AND NOT when the source IP is contained in Approved_Mail_Relays   │
│                                                                        │
│ ⚙️ RULE ACTIONS:                                                       │
│    • Ensure detected event is part of an OFFENSE                       │
│    • Index Offense by: Source IP                                       │
│    • Set Severity to: 8, Credibility to: 9, Relevance to: 8            │
│                                                                        │
│ 📢 RULE RESPONSES:                                                     │
│    • Add Source IP to Reference Set: "Active_Attacker_IPs" (TTL: 24h)  │
│    • Execute Custom Action: "soar_block_firewall_ip.py"                │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 📌 6. Visual Mind Map & Sticky Notes (Chapter 53 Quick-Review)

### 🧠 Hierarchical Mind Map
```mermaid
mindmap
  root((Brute Force Detection))
    Attack Types
      Vertical: 1 User, 100s of passwords
      Horizontal (Spraying): 1 Password, 100s of users
      Account Lockout: Win 4740 / Entra 50053
    Key Telemetry
      Windows: 4625 (Fail), 4624 (Success), 4740 (Lockout)
      Linux: sshd failed password
      Cloud: Entra ID 50126
    Rule Engineering
      Vertical Rule: Same SrcIP, >= 15 events in 3 min
      Spraying Rule: Same SrcIP, DIFFERENT Usernames, >= 20 in 10 min
      Response: Add to Active_Attacker_IPs RefSet
```

### 🗂️ Color-Coded Digital Sticky Notes

#### 🟨 Sticky Note 1: Spraying Requires "Different" Test
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 CONCEPT: THE "DIFFERENT USERNAME" TEST                   │
│                                                             │
│ • Password spraying detection REQUIRES the test:            │
│   "with the same Source IP and DIFFERENT Username".         │
└─────────────────────────────────────────────────────────────┘
```

#### 🟩 Sticky Note 2: Account Lockout Correlation
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 GOLDEN RULE: MULTI-EVENT CHAINING                        │
│                                                             │
│ • Correlate Event 4625 (Failures) $\rightarrow$ Event 4740  │
│   (Lockout) within 60 seconds on the same Username!         │
└─────────────────────────────────────────────────────────────┘
```

#### 🟪 Sticky Note 3: SubStatus 0xC000006A vs 0xC0000064
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 FORENSICS: WINDOWS 4625 SUBSTATUS CODES                  │
│                                                             │
│ • `0xC000006A` = Bad Password (Valid user exists!).         │
│ • `0xC0000064` = User Name does not exist (Enumeration!).   │
└─────────────────────────────────────────────────────────────┘
```

#### 🟦 Sticky Note 4: Reference Set Integration
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 AUTOMATION: ADD TO REFERENCE SET                         │
│                                                             │
│ • When a brute force rule fires, automatically add the      │
│   Source IP to an `Active_Attacker_IPs` Reference Set.      │
└─────────────────────────────────────────────────────────────┘
```

#### 🟧 Sticky Note 5: Interview Cheat Code (Brute Force Detection)
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 INTERVIEW CHEAT CODE: "HOW DO YOU DETECT PASSWORD SPRAY?"│
│                                                             │
│ "In QRadar, I configure an Event Rule testing for           │
│ Authentication Failure categories, aggregating on the same  │
│ Source IP with DIFFERENT Usernames over a sliding 10-minute │
│ window, indexing on Source IP with Magnitude 8."            │
└─────────────────────────────────────────────────────────────┘
```

#### 🟥 Sticky Note 6: Critical Pitfall
```text
┌─────────────────────────────────────────────────────────────┐
│ ⚠️ CRITICAL PITFALL: OUTLOOK / EXCHANGE PROXIES             │
│                                                             │
│ • Failed mobile ActiveSync connections make Exchange Client │
│   Access Servers appear as the brute force source! Ensure   │
│   X-Forwarded-For or client IP is parsed.                   │
└─────────────────────────────────────────────────────────────┘
```

---

## ⚡ Rapid-Recall Flashcards

| # | Question (Front) | Answer (Back) |
| :-: | :--- | :--- |
| **01** | What Windows Event ID indicates a failed logon attempt? | **Event ID 4625**. |
| **02** | What Windows Event ID indicates a user account lockout? | **Event ID 4740**. |
| **03** | What is the difference between vertical brute force and password spraying? | Vertical tests many passwords on 1 account; Spraying tests 1 common password across hundreds of different accounts. |
| **04** | What rule test is essential for detecting password spraying in QRadar? | `when at least X events occur with the same Source IP and DIFFERENT Username in Y minutes`. |
| **05** | What does Windows 4625 SubStatus `0xC000006A` indicate? | **Bad Password** (the account name is valid). |
| **06** | What automated action can QRadar take against an attacker IP detected by a brute force rule? | Add the IP to a dynamic **Reference Set** (e.g. `Active_Attacker_IPs`) for firewall blocking. |

---

## 🧪 7. Hands-on Practice Lab 53: Building & Emulating a Brute Force Detection Pipeline

### 🎯 Lab Objectives
1. Query authentication failure baseline events via AQL.
2. Emulate an SSH / AD brute-force attack stream using `logger`.
3. Verify rule matching and offense creation via the CLI.
4. Inspect the generated offense and contributing event list.

---

### 📋 Step-by-Step Practical Lab Procedures

#### Step 1: Query Baseline Authentication Failures via AQL
```bash
/opt/qradar/bin/arielsql -p 32011 -q "
SELECT 
    DATEFORMAT(starttime, 'HH:mm:ss') AS Time, 
    sourceip AS AttackerIP, 
    username AS TargetUser, 
    destinationip AS TargetServer, 
    COUNT(*) AS FailCount 
FROM events 
WHERE qidname(qid) ILIKE '%Login Failed%' OR qidname(qid) ILIKE '%Logon Failure%' 
LAST 24 HOURS 
GROUP BY sourceip, username, destinationip 
ORDER BY FailCount DESC 
LIMIT 10;
"
```

---

#### Step 2: Emulate a Multi-Event SSH Brute Force Attack via CLI
Inject 20 synthetic SSH login failure syslog packets into `ecs-ec-ingress`:
```bash
for i in {1..20}; do
    logger -n 127.0.0.1 -P 514 -d -t sshd \
    "<134>$(date '+%b %d %H:%M:%S') srv-app-01 sshd[$$]: Failed password for invalid user admin from 198.51.100.77 port $((50000 + i)) ssh2"
    sleep 0.2
done
```

---

#### Step 3: Monitor Live Rule Trigger in `/var/log/qradar.log`
```bash
grep -iE "CRE: Rule.*Brute Force|Offense ID" /var/log/qradar.log | tail -n 10
```

---

#### Step 4: Verify Generated Offense via PostgreSQL
```bash
psql -U qradar -d qradar -x -c "
SELECT 
    id AS OffenseID, 
    description, 
    offensesource, 
    eventcount, 
    magnitude, 
    severity, 
    credibility 
FROM offense 
WHERE offensesource = '198.51.100.77' 
ORDER BY id DESC 
LIMIT 1;
"
```
