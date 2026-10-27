# 📖 Chapter 67: Advanced AQL Aggregations: `COUNT`, `UNIQUECOUNT`, `SUM`, `AVG` & `TOP` Masterclass

> **The IBM QRadar Administrator's Technical Guide** | Phase 7: AQL Mastery & Ariel Storage Optimization  
> **Target Audience:** 1.5-Year Administrators $\rightarrow$ Senior SIEM Engineers & Detection Engineering Specialists  
> **Focus:** Aggregate Functions, `UNIQUECOUNT` HyperLogLog Mechanics, Multi-Dimensional `GROUP BY`, Time-Bucketing, & Hands-on Lab 67

---

## 📸 1. Visual Overview: Distributed Aggregation & MapReduce Processing

![AQL Aggregation Engine|1000](../assets/qradar_aql_query_engine_3d_1791647183664.jpg)
*Figure 67.1: Distributed Columnar Aggregation Pipeline in AQL: Worker Data Nodes calculating local intermediate counts/sums, streaming partial buckets to the Console Reducer for global sorting and multi-dimensional grouping (`![[qradar_aql_query_engine_3d_1791647183664.jpg|1000]]`)*

---

## 2. The Core Aggregate Functions in AQL

Aggregation functions collapse millions of discrete event or flow records into summarized analytical data structures:

```
┌────────────────────────────────────────────────────────────────────────┐
│ 📊 THE 6 PILLARS OF AQL AGGREGATION                                    │
│                                                                        │
│ 1. `COUNT(*)` $\rightarrow$ Total volume of records in group bucket.    │
│ 2. `UNIQUECOUNT(field)` $\rightarrow$ Cardinality of distinct items.   │
│ 3. `SUM(numeric_field)` $\rightarrow$ Total bytes, packets, durations. │
│ 4. `AVG(numeric_field)` $\rightarrow$ Mean value / mathematical average│
│ 5. `MIN(field)` & `MAX(field)` $\rightarrow$ Attack timeline bounds.   │
│ 6. `PERCENTILE(N, field)` $\rightarrow$ 90th/95th/99th percentile stats│
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Comprehensive Aggregation Function Matrix

| Function | Return Type | Applicable DB | Computational Cost | Primary Security Use Case |
| :--- | :---: | :---: | :---: | :--- |
| **`COUNT(*)`** | Long | Events & Flows | $\mathcal{O}(1)$ | Volumetric threat detection (DDoS, Brute Force hit count). |
| **`UNIQUECOUNT(field)`** | Long | Events & Flows | $\mathcal{O}(\text{Cardinality})$ | Port scan fan-out (Unique DstPorts), Password spraying (Unique Users). |
| **`SUM(sourcebytes)`** | Double / Long | Flows | $\mathcal{O}(1)$ | Data exfiltration volume calculations (MB / GB transferred). |
| **`AVG(duration)`** | Double | Flows | $\mathcal{O}(1)$ | Identifying long-lived covert C2 beacon sessions vs normal HTTP. |
| **`MIN(starttime)`** | Timestamp | Events & Flows | $\mathcal{O}(1)$ | Determining the exact initial second an attack began. |
| **`MAX(endtime)`** | Timestamp | Events & Flows | $\mathcal{O}(1)$ | Determining the last observed activity of an adversary. |

---

## 4. Multi-Dimensional Grouping & Time-Bucketing

### A. Multi-Dimensional Grouping (The 3-Tuple Pivot)
Grouping by multiple fields creates a hierarchical pivot table:

```sql
SELECT 
    sourceip AS AttackerIP,
    destinationip AS TargetServer,
    qidname(qid) AS AttackType,
    COUNT(*) AS EventHits,
    UNIQUECOUNT(username) AS TargetedUserCount
FROM events
WHERE hasoffense = TRUE
LAST 24 HOURS
GROUP BY sourceip, destinationip, qidname(qid)
HAVING EventHits >= 25
ORDER BY EventHits DESC;
```

### B. Time-Bucket Aggregation (Hourly & Daily Trend Series)
By formatting `starttime` inside `GROUP BY`, AQL transforms raw logs into hourly or daily time-series histograms:

```sql
SELECT 
    DATEFORMAT(starttime, 'YYYY-MM-dd HH:00') AS HourlyBucket,
    logsourceid,
    COUNT(*) AS EventsPerHour,
    UNIQUECOUNT(sourceip) AS UniqueActiveHosts
FROM events
LAST 7 DAYS
GROUP BY HourlyBucket, logsourceid
ORDER BY HourlyBucket ASC;
```

---

## 5. `UNIQUECOUNT` HyperLogLog Approximation vs Exact Counting

In large enterprise datasets ($> 50\text{M}$ records), calculating exact unique cardinality across strings requires gigabytes of memory. QRadar utilizes an optimized **HyperLogLog (HLL)** probabilistic algorithm:

```
┌────────────────────────────────────────────────────────────────────────┐
│ 🔬 HYPERLOGLOG (HLL) IN QRADAR AQL                                     │
│                                                                        │
│ • Cardinality $\le 5,000$ Unique Items:                                │
│   QRadar maintains an exact distinct hash set (100.00% precision).     │
│                                                                        │
│ • Cardinality $> 5,000$ Unique Items:                                  │
│   Switches to HyperLogLog register buckets, maintaining $\pm 1.5\%$    │
│   standard error while reducing RAM usage from 500 MB down to 12 KB!   │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 📌 6. Visual Mind Map & Sticky Notes (Chapter 67 Quick-Review)

### 🧠 Hierarchical Mind Map
```mermaid
mindmap
  root((AQL Aggregations))
    Functions
      COUNT(*): Total record frequency
      UNIQUECOUNT: Distinct cardinality (HLL)
      SUM / AVG: Flow bytes & duration
      MIN / MAX: Attack timeline boundaries
    Grouping Patterns
      Multi-dimensional 3-tuple pivots
      Time-series hourly/daily bucketing
    Filtering Aggregates
      HAVING clause (post-aggregation)
      HAVING UNIQUECOUNT(user) > 10
```

### 🗂️ Color-Coded Digital Sticky Notes

#### 🟨 Sticky Note 1: UNIQUECOUNT vs COUNT
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 CONCEPT: CARDINALITY VS VOLUME                           │
│                                                             │
│ • `COUNT(*)` counts total rows.                             │
│ • `UNIQUECOUNT(sourceip)` counts distinct IP addresses.     │
└─────────────────────────────────────────────────────────────┘
```

#### 🟩 Sticky Note 2: HAVING vs WHERE
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 GOLDEN RULE: FILTERING BEFORE VS AFTER                   │
│                                                             │
│ • Filter raw events BEFORE grouping with `WHERE`.           │
│ • Filter aggregated metric counts AFTER grouping with       │
│   `HAVING`.                                                 │
└─────────────────────────────────────────────────────────────┘
```

#### 🟪 Sticky Note 3: Converting Bytes to MB/GB
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 SYNTAX: READABLE FLOW BANDWIDTH                          │
│                                                             │
│ • `SUM(sourcebytes) / (1024*1024) AS TotalMBSent`           │
│ • `SUM(sourcebytes) / (1024*1024*1024) AS TotalGBSent`      │
└─────────────────────────────────────────────────────────────┘
```

#### 🟦 Sticky Note 4: Hourly Bucketing Syntax
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 TIME-SERIES: DATEFORMAT IN GROUP BY                      │
│                                                             │
│ • `GROUP BY DATEFORMAT(starttime, 'YYYY-MM-dd HH:00')`      │
│   creates clean hourly distribution tables for dashboards.  │
└─────────────────────────────────────────────────────────────┘
```

#### 🟧 Sticky Note 5: Interview Cheat Code (AQL Aggregations)
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 INTERVIEW CHEAT CODE: "HOW DO YOU DETECT PASSWORD SPRAY?"│
│                                                             │
│ "In AQL, I aggregate by Source IP and use `HAVING`          │
│ `UNIQUECOUNT(username) > 20` over failed logon QIDs to      │
│ identify single IPs spraying passwords across accounts."    │
└─────────────────────────────────────────────────────────────┘
```

#### 🟥 Sticky Note 6: Critical Pitfall
```text
┌─────────────────────────────────────────────────────────────┐
│ ⚠️ CRITICAL PITFALL: GROUPING UNINDEXED PAYLOAD STRINGS     │
│                                                             │
│ • Grouping by raw unparsed payload strings forces the engine│
│   to allocate massive heap hash buckets, causing slow query │
│   timeouts. Group by normalized integer/IP fields!          │
└─────────────────────────────────────────────────────────────┘
```

---

## ⚡ Rapid-Recall Flashcards

| # | Question (Front) | Answer (Back) |
| :-: | :--- | :--- |
| **01** | What AQL function calculates the count of distinct values in a column? | **`UNIQUECOUNT(column_name)`**. |
| **02** | What underlying probabilistic algorithm does QRadar use for fast large-scale distinct counting? | **HyperLogLog (HLL)**. |
| **03** | How do you filter out groups with fewer than 100 events in an AQL aggregation query? | Use **`HAVING COUNT(*) >= 100`** after the `GROUP BY` clause. |
| **04** | How do you calculate total Megabytes transferred in an AQL flow query? | **`SUM(sourcebytes) / 1048576`** (or `SUM(sourcebytes) / (1024*1024)`). |
| **05** | How do you group events into 1-hour time buckets in AQL? | **`GROUP BY DATEFORMAT(starttime, 'YYYY-MM-dd HH:00')`**. |
| **06** | What functions determine the exact start and end boundaries of an attack sequence? | **`MIN(starttime)`** and **`MAX(endtime)`**. |

---

## 🧪 7. Hands-on Practice Lab 67: Writing Multi-Dimensional Aggregation Queries

### 🎯 Lab Objectives
1. Build a horizontal password spraying detection query using `UNIQUECOUNT`.
2. Construct an hourly time-series event distribution histogram.
3. Calculate network flow bandwidth consumption per destination server.
4. Apply multi-condition `HAVING` filters.

---

### 📋 Step-by-Step Practical Lab Procedures

#### Step 1: Detect Distributed Password Spraying via `UNIQUECOUNT`
Log into the QRadar Console via SSH as `root`:
```bash
/opt/qradar/bin/arielsql -p 32011 -q "
SELECT 
    sourceip AS AttackerIP, 
    UNIQUECOUNT(username) AS UniqueUsersTargeted, 
    COUNT(*) AS TotalFailedAttempts, 
    MIN(starttime) AS FirstAttempt, 
    MAX(endtime) AS LastAttempt 
FROM events 
WHERE qidname(qid) ILIKE '%Failed%' OR qidname(qid) ILIKE '%Logon Failure%' 
LAST 24 HOURS 
GROUP BY sourceip 
HAVING UniqueUsersTargeted >= 5 
ORDER BY UniqueUsersTargeted DESC;
"
```

---

#### Step 2: Build an Hourly Event Volume Time-Series Histogram
```bash
/opt/qradar/bin/arielsql -p 32011 -q "
SELECT 
    DATEFORMAT(starttime, 'YYYY-MM-dd HH:00') AS HourWindow, 
    COUNT(*) AS TotalEvents, 
    UNIQUECOUNT(sourceip) AS ActiveSourceHosts 
FROM events 
LAST 24 HOURS 
GROUP BY HourWindow 
ORDER BY HourWindow ASC;
"
```

---

#### Step 3: Analyze Flow Bandwidth by Application & Destination Server
```bash
/opt/qradar/bin/arielsql -p 32011 -q "
SELECT 
    destinationip AS TargetServer, 
    destinationport AS DstPort, 
    SUM(sourcebytes) / (1024*1024) AS TotalMBSent, 
    SUM(destinationbytes) / (1024*1024) AS TotalMBReceived, 
    AVG(duration) / 1000 AS AvgSessionDurationSec, 
    COUNT(*) AS SessionCount 
FROM flows 
LAST 12 HOURS 
GROUP BY destinationip, destinationport 
HAVING TotalMBSent > 50 
ORDER BY TotalMBSent DESC 
LIMIT 15;
"
```
