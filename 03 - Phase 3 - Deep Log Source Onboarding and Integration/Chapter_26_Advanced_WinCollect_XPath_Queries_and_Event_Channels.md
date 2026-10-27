# 📖 Chapter 26: Advanced WinCollect: XPath Queries & Event Channels Masterclass

> **The IBM QRadar Administrator's Technical Guide** | Phase 3: Deep Log Source Onboarding & Integration  
> **Target Audience:** 1.5-Year Administrators $\rightarrow$ Senior SIEM Engineers & Threat Hunting Specialists  
> **Focus:** Windows Crimson Channels (Sysmon, PowerShell 4104), Advanced XPath XML Filtering, Noise Suppression, & Hands-on Lab 26

---

## 📸 1. Visual Overview: Windows Telemetry & XPath Filtering Engine

![WinCollect XPath Filtering Architecture|1000](../assets/16_asset_profiler_identity_model_3d.jpg)
*Figure 26.1: 3D Isometric View of Advanced WinCollect Endpoint Collection: Native Windows Event Subsystem streaming Security, Sysmon, and PowerShell channels into the in-memory WinCollect XPath Filter Engine, dropping high-volume noise (Event IDs 5156, 4689) at the endpoint while transmitting high-fidelity threat indicators over TLS 6514 (`![[16_asset_profiler_identity_model_3d.jpg|1000]]`)*

---

## 2. The Power of Agent-Side XPath XML Filtering

In standard Windows logging, **up to 70% of generated Security events represent benign noise** (e.g., Windows Filtering Platform network connections or background service tickets).

```
┌────────────────────────────────────────────────────────────────────────┐
│ 🚀 WHY XPATH FILTERING AT THE AGENT LEVEL SAVES THOUSANDS IN LICENSING│
│                                                                        │
│ ❌ WITHOUT XPATH FILTERING (Naive Ingestion):                           │
│ • Domain Controller generates: 8,000 EPS.                              │
│ • 5,500 EPS consists of noisy Event ID 5156 (Firewall connection).     │
│ • All 8,000 EPS transmitted across WAN $\rightarrow$ Consumes SIEM EPS license!│
│ ├────────────────────────────────────────────────────────────────────┤
│ ✅ WITH XPATH FILTERING (Smart Ingestion):                              │
│ • WinCollect applies XPath filter directly in Windows RAM.             │
│ • Drops Event ID 5156 immediately before network serialization.        │
│ • Only 2,500 high-fidelity EPS sent to QRadar $\rightarrow$ 68% License Saving! │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Advanced Windows Channels: Beyond Standard Security Logs

Modern threat detection requires collecting from dedicated **Windows Crimson Channels**:

| Channel Name | Critical Event IDs | Detection Capability |
| :--- | :--- | :--- |
| **`Security`** | 4624 (Logon), 4625 (Failed Logon), 4688 (Process Creation), 4720 (User Created), 4728 (Group Add) | Core authentication, privilege escalation, and account creation. |
| **`Microsoft-Windows-Sysmon/Operational`** | 1 (Process Create + CLI), 3 (Network Connect), 7 (Image Load), 10 (ProcessAccess / Mimikatz), 11 (FileCreate) | Comprehensive endpoint threat hunting and process genealogy. |
| **`Microsoft-Windows-PowerShell/Operational`** | 4104 (Script Block Logging), 4103 (Module Logging) | Obfuscated PowerShell execution, fileless malware, Living-off-the-Land (LOLBins). |
| **`Microsoft-Windows-TaskScheduler/Operational`** | 106 (Task Scheduled), 200 (Action Executed) | Persistence mechanisms and scheduled task execution. |
| **`Microsoft-Windows-TerminalServices-LocalSessionManager/Operational`** | 21 (Session Logon), 24 (Session Disconnected), 25 (Session Reconnected) | Lateral movement via Remote Desktop Protocol (RDP). |

---

## 4. Writing Enterprise XPath XML Queries

XPath queries are configured directly inside the WinCollect Log Source settings in QRadar:

### 🎯 1. Inclusive XPath Query: High-Fidelity Security + Sysmon
Collects only critical authentication, process execution, and Sysmon events:

```xml
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">
      *[System[(EventID=4624 or EventID=4625 or EventID=4688 or EventID=4720 or EventID=4726 or EventID=4728 or EventID=4738 or EventID=4776 or EventID=7045)]]
    </Select>
  </Query>
  <Query Id="1" Path="Microsoft-Windows-Sysmon/Operational">
    <Select Path="Microsoft-Windows-Sysmon/Operational">
      *[System[(EventID=1 or EventID=3 or EventID=7 or EventID=8 or EventID=10 or EventID=11 or EventID=13 or EventID=22)]]
    </Select>
  </Query>
</QueryList>
```

---

### 🛡️ 2. Suppressive XPath Query: Exclude High-Volume Noise
Collects ALL Security events *except* noisy network filter and process termination events:

```xml
<QueryList>
  <Query Id="0" Path="Security">
    <Select Path="Security">*</Select>
    <Suppress Path="Security">
      *[System[(EventID=5156 or EventID=5157 or EventID=5158 or EventID=4689 or EventID=4662 or EventID=4690)]]
    </Suppress>
  </Query>
  <Query Id="1" Path="Microsoft-Windows-PowerShell/Operational">
    <Select Path="Microsoft-Windows-PowerShell/Operational">
      *[System[(EventID=4104)]]
    </Select>
  </Query>
</QueryList>
```

---

## 📌 5. Visual Mind Map & Sticky Notes (Chapter 26 Quick-Review)

### 🧠 Hierarchical Mind Map
```mermaid
mindmap
  root((Advanced WinCollect))
    Windows Channels
      Security: Auth, User management, 4688
      Sysmon: Process trees (EID 1), Mimikatz (EID 10)
      PowerShell: Script Block Logging (EID 4104)
      TaskScheduler: Scheduled task persistence
    XPath Filtering
      Location: Executed in Windows RAM before network
      Inclusive: Select only critical IDs (4624, 4688, 1)
      Suppressive: Suppress noisy IDs (5156, 4689)
    Enterprise Benefit
      60-70% EPS License & Storage savings
```

### 🗂️ Color-Coded Digital Sticky Notes

#### 🟨 Sticky Note 1: Endpoint Filtering Principle
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 CONCEPT: THE XPATH ADVANTAGE                             │
│                                                             │
│ • Filtering logs at the endpoint reduces network bandwidth, │
│   saves QRadar EPS license capacity, and lowers storage IOPS│
│ • Filter at the edge, not at the core!                      │
└─────────────────────────────────────────────────────────────┘
```

#### 🟩 Sticky Note 2: Sysmon Event ID 1 & 10
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 GOLDEN RULE: SYSMON THREAT HUNTING                       │
│                                                             │
│ • Sysmon Event ID 1 captures full command line arguments.   │
│ • Sysmon Event ID 10 captures LSASS process memory access   │
│   (Instant Mimikatz credential dumping detection!).         │
└─────────────────────────────────────────────────────────────┘
```

#### 🟪 Sticky Note 3: PowerShell Script Block (4104)
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 ARCHITECTURE: DE-OBFUSCATION LOGGING                     │
│                                                             │
│ • Event ID 4104 records the DE-OBFUSCATED script content at │
│   runtime, defeating Base64 and variable obfuscation!       │
└─────────────────────────────────────────────────────────────┘
```

#### 🟦 Sticky Note 4: Suppressing Event ID 5156
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 VALUE: NOISE SUPPRESSION                                 │
│                                                             │
│ • Event ID 5156 (Filtering Platform) generates millions of  │
│   records for normal browser traffic.                       │
│ • ALWAYS suppress Event ID 5156 via XPath!                  │
└─────────────────────────────────────────────────────────────┘
```

#### 🟧 Sticky Note 5: Interview Cheat Code (XPath Optimization)
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 INTERVIEW CHEAT CODE: "HOW DO YOU OPTIMIZE WINDOWS EPS?" │
│                                                             │
│ "I configure WinCollect with custom XPath XML queries at the│
│ endpoint to collect high-value Security, Sysmon, and        │
│ PowerShell 4104 channels while suppressing noisy Event IDs  │
│ like 5156 and 4689, cutting ingestion noise by up to 70%."  │
└─────────────────────────────────────────────────────────────┘
```

#### 🟥 Sticky Note 6: Critical Pitfall
```text
┌─────────────────────────────────────────────────────────────┐
│ ⚠️ CRITICAL PITFALL: BROKEN XPATH SYNTAX                    │
│                                                             │
│ • A single missing XML tag in an XPath `<QueryList>` causes │
│   WinCollect to silently fail channel polling!              │
│ • Always test XPath queries in Windows Event Viewer first!  │
└─────────────────────────────────────────────────────────────┘
```

---

### ⚡ Rapid-Recall Flashcards

| # | Question (Front) | Answer (Back) |
| :-: | :--- | :--- |
| **01** | Where are WinCollect XPath queries executed? | **Directly on the Windows endpoint in memory** before sending logs over the network. |
| **02** | Which Windows PowerShell event records de-obfuscated script code execution? | **Event ID 4104 (Script Block Logging)**. |
| **03** | Which Sysmon event detects credential dumping tools accessing LSASS memory? | **Sysmon Event ID 10 (ProcessAccess)**. |
| **04** | Which noisy Windows event ID should almost always be suppressed? | **Event ID 5156 (Windows Filtering Platform Connection Permitted)**. |
| **05** | What XML tag is used in an XPath query to exclude specific Event IDs? | The **`<Suppress>`** tag. |
| **06** | What Windows channel records scheduled task execution? | **`Microsoft-Windows-TaskScheduler/Operational`**. |

---

## 🧪 6. Hands-on Practice Lab 26: Authoring & Validating XPath XML Filters

### 🎯 Lab Objectives
1. Construct structured inclusive and suppressive XPath XML queries.
2. Validate XPath syntax against Windows Event Log channels.
3. Configure WinCollect channel subscriptions for Sysmon and PowerShell.
4. Verify event filtering efficiency and noise reduction in QRadar Log Activity.

---

### 📋 Step-by-Step Practical Lab Procedures

#### Step 1: Create Production XPath XML Filter Template
Save the following validated enterprise XPath configuration file:
```xml
cat << 'EOF' > /tmp/wincollect_production_xpath.xml
<QueryList>
  <!-- 1. High-Value Windows Security Events -->
  <Query Id="0" Path="Security">
    <Select Path="Security">
      *[System[(EventID=4624 or EventID=4625 or EventID=4648 or EventID=4688 or EventID=4720 or EventID=4726 or EventID=4728 or EventID=4738 or EventID=4776 or EventID=7045)]]
    </Select>
  </Query>
  <!-- 2. Sysmon Endpoint Threat Hunting -->
  <Query Id="1" Path="Microsoft-Windows-Sysmon/Operational">
    <Select Path="Microsoft-Windows-Sysmon/Operational">
      *[System[(EventID=1 or EventID=3 or EventID=7 or EventID=10 or EventID=11 or EventID=22)]]
    </Select>
  </Query>
  <!-- 3. PowerShell Script Block Logging -->
  <Query Id="2" Path="Microsoft-Windows-PowerShell/Operational">
    <Select Path="Microsoft-Windows-PowerShell/Operational">
      *[System[(EventID=4104)]]
    </Select>
  </Query>
</QueryList>
EOF
```

---

#### Step 2: Audit Windows Event Ingestion via AQL
From the QRadar Console CLI, run an AQL query to verify the top Windows Event IDs arriving:
```bash
/opt/qradar/bin/arielsql -p 32011 -q "
SELECT 
    qidname(qid) AS EventName, 
    sourceip, 
    COUNT(*) AS EventCount 
FROM events 
WHERE devicetypeid = 12 
GROUP BY EventName, sourceip 
ORDER BY EventCount DESC 
LAST 2 HOURS;
"
```
*Expected Output*: Confirms ingestion of high-value authentication and process events with zero Event ID 5156 noise.
