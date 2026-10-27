# 📖 Chapter 68: AQL Function Library: String, Network, Math & Date Masterclass

> **The IBM QRadar Administrator's Technical Guide** | Phase 7: AQL Mastery & Ariel Storage Optimization  
> **Target Audience:** 1.5-Year Administrators $\rightarrow$ Senior SIEM Engineers & Detection Engineering Specialists  
> **Focus:** Built-in AQL Functions, CIDR Matching (`INCIDR`), GeoIP Enrichment, Date Formatting, & Hands-on Lab 68

---

## 📸 1. Visual Overview: AQL In-Flight Transformation & Enrichment Library

![AQL Function Library|1000](../assets/qradar_aql_query_engine_3d_1791647183664.jpg)
*Figure 68.1: AQL Real-Time Data Transformation Pipeline: Transforming raw data types using String regex manipulators, MaxMind GeoIP resolvers, CIDR Radix tree matchers, and Reference Data in-memory hash functions (`![[qradar_aql_query_engine_3d_1791647183664.jpg|1000]]`)*

---

## 2. The 5 Core Function Families in AQL

AQL provides a comprehensive library of scalar and transformation functions evaluated on the fly during query execution:

```
┌────────────────────────────────────────────────────────────────────────┐
│ 📚 THE 5 AQL SCALAR FUNCTION FAMILIES                                  │
│                                                                        │
│ 1. NETWORK & IP FUNCTIONS (`INCIDR`, `GEOIP`, `LONGTOIP`, `ASNUMBER`)  │
│ 2. DATE & TIME FUNCTIONS (`DATEFORMAT`, `NOW`, `TIMEDIFF`)             │
│ 3. STRING FUNCTIONS (`CONCAT`, `LOWER`, `SUBSTRING`, `STRLEN`, `UTF8`) │
│ 4. REFERENCE DATA FUNCTIONS (`REFERENCESETCONTAINS`, `REFERENCEMAP`)   │
│ 5. MATHEMATICAL & CONVERSION FUNCTIONS (`ROUND`, `FLOOR`, `INT`)       │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Network, IP & Geo-Location Functions

Network functions allow instant subnet masking and geopolitical threat enrichment:

| Function | Return Type | Description & Syntax Example |
| :--- | :---: | :--- |
| **`INCIDR('CIDR', IP)`** | Boolean | Checks if an IP resides within a subnet.<br/>`WHERE INCIDR('192.168.0.0/16', destinationip)` |
| **`GEOIP(IP, 'Attribute')`** | String | MaxMind GeoIP database lookup in RAM.<br/>Attributes: `'country_name'`, `'country_code'`, `'city'`, `'latitude'`, `'longitude'`.<br/>`SELECT GEOIP(sourceip, 'country_name') AS Country` |
| **`ASNUMBER(IP)`** | Long | Returns the Autonomous System Number (ASN) of the IP.<br/>`SELECT ASNUMBER(sourceip) AS ASN` |
| **`LONGTOIP(LongVal)`** | String | Converts integer-encoded IP back to dotted decimal.<br/>`SELECT LONGTOIP(sourceip) AS DottedIP` |
| **`IPTONAME(IP)`** | String | Resolves IP to hostname via Asset Profiler cache.<br/>`SELECT IPTONAME(destinationip) AS AssetName` |

---

## 4. Date & Time Transformation Functions

| Function | Syntax & Pattern | Practical Output |
| :--- | :--- | :--- |
| **`DATEFORMAT(timestamp, format)`** | `DATEFORMAT(starttime, 'YYYY-MM-dd HH:mm:ss')`<br/>`DATEFORMAT(starttime, 'E, MMM dd YYYY')` | `2026-09-25 14:30:00`<br/>`Fri, Sep 25 2026` |
| **`NOW()`** | `NOW()` | Returns current epoch time in milliseconds. |
| **`TIMEDIFF(t1, t2, 'unit')`** | `TIMEDIFF(endtime, starttime, 'seconds')` | Calculates duration difference between timestamps. |

---

## 5. String Manipulation & Raw Payload Decoders

| Function | Syntax Example | Practical Output |
| :--- | :--- | :--- |
| **`UTF8(payload)`** | `UTF8(payload)` | Converts binary-compressed raw payload bytes into readable UTF-8 text string. |
| **`CONCAT(s1, s2, ...)`** | `CONCAT(username, '@', \"Domain\")` | `jsmith@corp.local` |
| **`LOWER(str)` / `UPPER(str)`** | `LOWER(username)` | `admin` (normalizes case). |
| **`SUBSTRING(str, start, len)`** | `SUBSTRING(\"ProcessName\", 0, 15)` | Extracts leading 15 characters. |
| **`STRLEN(str)`** | `STRLEN(\"DNS_QueryName\")` | Returns integer length (used for DNS tunneling hunts). |
| **`REPLACEALL(regex, repl, str)`**| `REPLACEALL('\\\\', '/', filepath)` | Sanitizes Windows backslashes into forward slashes. |

---

## 6. Comprehensive Multi-Function Forensic Hunting Query

```sql
SELECT 
    DATEFORMAT(starttime, 'YYYY-MM-dd HH:mm:ss') AS AttackTime,
    sourceip AS ExternalAttackerIP,
    GEOIP(sourceip, 'country_name') AS AttackerCountry,
    GEOIP(sourceip, 'city') AS AttackerCity,
    ASNUMBER(sourceip) AS AttackerASN,
    destinationip AS TargetedServer,
    IPTONAME(destinationip) AS TargetHostName,
    destinationport AS TargetPort,
    LOWER(username) AS TargetAccount,
    STRLEN(UTF8(payload)) AS RawPayloadLength,
    qidname(qid) AS EventSignature
FROM events
WHERE NOT INCIDR('10.0.0.0/8', sourceip) 
  AND NOT INCIDR('172.16.0.0/12', sourceip)
  AND NOT INCIDR('192.168.0.0/16', sourceip)
  AND qidname(qid) ILIKE '%Exploit%'
LAST 24 HOURS
ORDER BY starttime DESC
LIMIT 25;
```

---

## 📌 7. Visual Mind Map & Sticky Notes (Chapter 68 Quick-Review)

### 🧠 Hierarchical Mind Map
```mermaid
mindmap
  root((AQL Functions))
    Network Functions
      INCIDR: CIDR subnet match
      GEOIP: Country, City, Lat/Long
      ASNUMBER: ISP / BGP Autonomous System
      IPTONAME: Asset cache hostname
    Date Functions
      DATEFORMAT: YYYY-MM-dd HH:mm:ss
      NOW: Current epoch timestamp
      TIMEDIFF: Duration calculation
    String & Payload
      UTF8(payload): Raw string decode
      CONCAT / LOWER / UPPER
      STRLEN & SUBSTRING
      REPLACEALL (regex)
```

### 🗂️ Color-Coded Digital Sticky Notes

#### 🟨 Sticky Note 1: Fast CIDR Matching
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 CONCEPT: INCIDR FUNCTION                                 │
│                                                             │
│ • `INCIDR('10.0.0.0/8', sourceip)` uses in-memory bitmask   │
│   radix trees for $\mathcal{O}(1)$ subnet evaluation!       │
└─────────────────────────────────────────────────────────────┘
```

#### 🟩 Sticky Note 2: MaxMind GeoIP Integration
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 GOLDEN RULE: GEOIP ENRICHMENT                            │
│                                                             │
│ • `GEOIP(sourceip, 'country_name')` enriches external IPs   │
│   instantly from the local MaxMind binary DB in RAM.        │
└─────────────────────────────────────────────────────────────┘
```

#### 🟪 Sticky Note 3: UTF8(payload) Cost
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 PERFORMANCE: LAZY PAYLOAD LOADING                        │
│                                                             │
│ • Calling `UTF8(payload)` forces disk reads of raw payload  │
│   files. Only use it when extracting unparsed text!         │
└─────────────────────────────────────────────────────────────┘
```

#### 🟦 Sticky Note 4: Case Normalization with LOWER
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 AGGREGATION: CLEAN USERNAME GROUPING                     │
│                                                             │
│ • Use `LOWER(username)` in `GROUP BY` to merge `JSmith` and │
│   `jsmith` into the same aggregate bucket!                  │
└─────────────────────────────────────────────────────────────┘
```

#### 🟧 Sticky Note 5: Interview Cheat Code (AQL Functions)
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 INTERVIEW CHEAT CODE: "HOW DO YOU FILTER PRIVATE SUBNETS?"│
│                                                             │
│ "In AQL, I use the `INCIDR` function with boolean NOT:      │
│ `WHERE NOT INCIDR('10.0.0.0/8', ip) AND NOT                 │
│ INCIDR('192.168.0.0/16', ip)` for high-speed filtering."    │
└─────────────────────────────────────────────────────────────┘
```

#### 🟥 Sticky Note 6: Critical Pitfall
```text
┌─────────────────────────────────────────────────────────────┐
│ ⚠️ CRITICAL PITFALL: GEOIP ON PRIVATE RFC1918 IPS           │
│                                                             │
│ • Calling `GEOIP()` on internal private IPs (`10.x.x.x`)    │
│   returns `NULL` or `'N/A'`. Filter for external IPs first! │
└─────────────────────────────────────────────────────────────┘
```

---

## ⚡ Rapid-Recall Flashcards

| # | Question (Front) | Answer (Back) |
| :-: | :--- | :--- |
| **01** | What AQL function checks if an IP address falls within a specific CIDR subnet block? | **`INCIDR('CIDR_Block', ip_field)`**. |
| **02** | What function extracts the geographic country name of an external IP in AQL? | **`GEOIP(sourceip, 'country_name')`**. |
| **03** | How do you convert compressed binary raw payload data into readable text in AQL? | **`UTF8(payload)`**. |
| **04** | How do you format timestamps into human-readable strings like `2026-09-25 14:00:00`? | **`DATEFORMAT(starttime, 'YYYY-MM-dd HH:mm:ss')`**. |
| **05** | What function converts an integer-encoded IP address back to dotted decimal format? | **`LONGTOIP(numeric_ip)`**. |
| **06** | What function calculates the length of a string in AQL? | **`STRLEN(string_field)`**. |

---

## 🧪 8. Hands-on Practice Lab 68: Advanced Forensic Query Construction with AQL Functions

### 🎯 Lab Objectives
1. Build an external threat geolocation query using `GEOIP` and `INCIDR`.
2. Normalize user identity strings using `LOWER` and `CONCAT`.
3. Inspect and extract raw strings from payloads using `UTF8(payload)`.
4. Measure session durations using `TIMEDIFF`.

---

### 📋 Step-by-Step Practical Lab Procedures

#### Step 1: Query External Attacker Geolocation & Subnet Filtering
Log into the QRadar Console via SSH as `root`:
```bash
/opt/qradar/bin/arielsql -p 32011 -q "
SELECT 
    DATEFORMAT(starttime, 'HH:mm:ss') AS Time, 
    sourceip AS ExternalAttacker, 
    GEOIP(sourceip, 'country_name') AS Country, 
    GEOIP(sourceip, 'city') AS City, 
    destinationip AS InternalTarget, 
    qidname(qid) AS EventName 
FROM events 
WHERE NOT INCIDR('10.0.0.0/8', sourceip) 
  AND NOT INCIDR('192.168.0.0/16', sourceip) 
  AND sourceip IS NOT NULL 
LAST 12 HOURS 
LIMIT 15;
"
```

---

#### Step 2: Normalize User Identity and Group Counts
```bash
/opt/qradar/bin/arielsql -p 32011 -q "
SELECT 
    LOWER(username) AS NormalizedUser, 
    COUNT(*) AS TotalAttempts, 
    UNIQUECOUNT(sourceip) AS UniqueSourceIPs 
FROM events 
WHERE username IS NOT NULL 
LAST 24 HOURS 
GROUP BY NormalizedUser 
ORDER BY TotalAttempts DESC 
LIMIT 10;
"
```

---

#### Step 3: Inspect Decoded Payloads for High-Risk Web Attacks
```bash
/opt/qradar/bin/arielsql -p 32011 -q "
SELECT 
    DATEFORMAT(starttime, 'YYYY-MM-dd HH:mm:ss') AS EventTime, 
    sourceip, 
    destinationip, 
    UTF8(payload) AS DecodedRawPayload 
FROM events 
WHERE destinationport IN (80, 443, 8080) 
  AND (qidname(qid) ILIKE '%SQL%' OR qidname(qid) ILIKE '%Injection%') 
LAST 24 HOURS 
LIMIT 5;
"
```
