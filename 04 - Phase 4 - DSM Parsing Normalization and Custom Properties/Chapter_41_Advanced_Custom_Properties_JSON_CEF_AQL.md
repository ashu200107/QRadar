# 📖 Chapter 41: Advanced Custom Properties: JSON, CEF & AQL Masterclass

> **The IBM QRadar Administrator's Technical Guide** | Phase 4: DSM Parsing, Normalization & Custom Properties  
> **Target Audience:** 1.5-Year Administrators $\rightarrow$ Senior SIEM Engineers & Cloud Detection Engineers  
> **Focus:** JSONPath Expressions, Nested JSON Extraction, CEF/LEEF Key Matching, AQL Calculated Properties, & Hands-on Lab 41

---

## 📸 1. Visual Overview: JSON & Structured Property Extraction

![Advanced Custom Property Architecture|1000](../assets/stage5_6_parsing_qid_3d.jpg)
*Figure 41.1: 3D Isometric View of the Advanced Property Extraction Subsystem: High-speed Jackson JSON Parser and CEF Key-Value Decoder extracting deeply nested Cloud attributes (`$.userIdentity.arn`) and security extension fields with zero complex regular expression overhead (`![[stage5_6_parsing_qid_3d.jpg|1000]]`)*

---

## 2. The Move to Structured Telemetry: JSON & CEF

While legacy logs relied on unstructured ASCII text, modern cloud providers (AWS, Azure, GCP, Kubernetes) and security platforms (CrowdStrike, Zscaler) emit structured **JSON** or **CEF (Common Event Format)** strings.

```
┌────────────────────────────────────────────────────────────────────────┐
│ 🚀 EXTRACTION METHOD PERFORMANCE COMPARISON                            │
│                                                                        │
│ 1. REGEX-BASED EXTRACTION:                                             │
│    • Slower on complex nested strings; high CPU on backtracking.       │
│    • Brittle: Breaks if JSON key order shifts!                         │
│ ├────────────────────────────────────────────────────────────────────┤
│ 2. JSONPATH-BASED EXTRACTION (Modern Recommended Standard):            │
│    • 5x faster than regex using QRadar's native Jackson JSON parser.   │
│    • Immune to key ordering changes; extracts deeply nested objects.   │
│ ├────────────────────────────────────────────────────────────────────┤
│ 3. CEF / LEEF KEY EXTRACTION:                                          │
│    • Native dictionary lookup on standardized keys (`cs1`, `deviceIn`).│
│    • Zero regex computation required!                                  │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 3. JSONPath Syntax & Deep Object Extraction

When extracting fields from AWS CloudTrail or Kubernetes JSON audit events, use standard **JSONPath expressions**:

```json
{
  "eventVersion": "1.08",
  "userIdentity": {
    "type": "IAMUser",
    "principalId": "AIDAJQABLZS4A3QEXAMPLE",
    "arn": "arn:aws:iam::123456789012:user/alice",
    "accountId": "123456789012"
  },
  "eventSource": "s3.amazonaws.com",
  "eventName": "PutObject",
  "requestParameters": {
    "bucketName": "finance-confidential-bucket",
    "key": "payroll_2026.xlsx"
  }
}
```

### 📋 JSONPath Expression Mapping Table

| Target Custom Property | JSONPath Expression | Extracted Value |
| :--- | :--- | :--- |
| **`AWS_IAM_ARN`** | `$.userIdentity.arn` | `arn:aws:iam::123456789012:user/alice` |
| **`AWS_Target_Bucket`** | `$.requestParameters.bucketName` | `finance-confidential-bucket` |
| **`AWS_Object_Key`** | `$.requestParameters.key` | `payroll_2026.xlsx` |
| **`AWS_Account_ID`** | `$.userIdentity.accountId` | `123456789012` |

---

## 4. CEF & LEEF Key-Based Custom Properties

Common Event Format (CEF) and Log Event Extended Format (LEEF) format extension fields as key-value pairs:

```text
CEF:0|Check Point|VPN-1 & FireWall-1|CheckPoint|Drop|Drop|0|act=drop src=198.51.100.22 dst=10.0.0.1 proto=6 spt=54120 dpt=22 cs1=Rule-10-DMZ cn1=452
```

In QRadar Custom Event Properties, select:
- **Extraction Type**: `CEF` (or `LEEF`)
- **Key Name**: `cs1` (or `cn1`)
- **Extracted Result**: `Rule-10-DMZ` (Instantly extracted without writing regex!).

---

## 5. AQL-Calculated Custom Properties (Search-Time Derivation)

Some properties cannot be extracted directly from a single field, but must be mathematically calculated or transformed at search time:

```sql
-- Calculate Total Network Bandwidth in Megabytes
SELECT 
    sourceip, 
    destinationip, 
    (sourcebytes + destinationbytes) / (1024 * 1024) AS TotalTrafficMB, 
    CONCAT(sourceip, ' -> ', destinationip) AS NetworkFlowVector, 
    LOWER(username) AS CleanUser 
FROM events 
WHERE TotalTrafficMB > 50 
LAST 2 HOURS;
```

---

## 📌 6. Visual Mind Map & Sticky Notes (Chapter 41 Quick-Review)

### 🧠 Hierarchical Mind Map
```mermaid
mindmap
  root((Advanced Properties))
    JSONPath
      Syntax: $.parent.child.target
      Engine: Jackson JSON parser in RAM
      Use Cases: AWS, Azure, K8s, Okta
    CEF / LEEF Keys
      Direct key lookup: cs1, cn2, act
      Zero regex CPU overhead
    AQL Derived
      Calculations: (bytes_in + bytes_out)
      String manipulation: CONCAT(), LOWER()
```

### 🗂️ Color-Coded Digital Sticky Notes

#### 🟨 Sticky Note 1: JSONPath vs Regex
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 CONCEPT: THE JSONPATH ADVANTAGE                          │
│                                                             │
│ • Always use JSONPath for JSON payloads.                    │
│ • If a cloud vendor adds 10 new JSON keys, JSONPath continues│
│   working without breaking!                                 │
└─────────────────────────────────────────────────────────────┘
```

#### 🟩 Sticky Note 2: Zero Regex on CEF
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 GOLDEN RULE: CEF KEY EXTRACTION                          │
│                                                             │
│ • When logs arrive in CEF/LEEF format, select "CEF Key" as  │
│   the extraction type in the DSM Editor.                    │
│ • Eliminates regex CPU consumption completely!              │
└─────────────────────────────────────────────────────────────┘
```

#### 🟪 Sticky Note 3: Nested JSON Array Syntax
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 ARCHITECTURE: ARRAY TARGETING                            │
│                                                             │
│ • Use `$.items[0].name` to extract the first element.       │
│ • Use `$.items[*].name` to extract all elements.            │
└─────────────────────────────────────────────────────────────┘
```

#### 🟦 Sticky Note 4: AQL Search-Time Flexibility
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 VALUE: DYNAMIC DERIVATIONS                               │
│                                                             │
│ • AQL calculations do not require creating permanent custom │
│   properties in PostgreSQL. Derive fields on-the-fly!       │
└─────────────────────────────────────────────────────────────┘
```

#### 🟧 Sticky Note 5: Interview Cheat Code (JSON Telemetry)
```text
┌─────────────────────────────────────────────────────────────┐
│ 📌 INTERVIEW CHEAT CODE: "HOW DO YOU PARSE CLOUD JSON LOGS?"│
│                                                             │
│ "I use JSONPath expressions (e.g. `$.userIdentity.arn`)     │
│ within QRadar Custom Event Properties, leveraging the native│
│ Jackson parser for 5x faster processing and zero regex lag."│
└─────────────────────────────────────────────────────────────┘
```

#### 🟥 Sticky Note 6: Critical Pitfall
```text
┌─────────────────────────────────────────────────────────────┐
│ ⚠️ CRITICAL PITFALL: REGEX ON JSON STRINGS                  │
│                                                             │
│ • Using regex like `"bucket":"(.*?)"` on JSON breaks when   │
│   whitespace, formatting, or nested sub-objects change!     │
└─────────────────────────────────────────────────────────────┘
```

---

### ⚡ Rapid-Recall Flashcards

| # | Question (Front) | Answer (Back) |
| :-: | :--- | :--- |
| **01** | What extraction type should be used for AWS CloudTrail and Kubernetes audit logs? | **JSONPath Expressions**. |
| **02** | What JSONPath expression extracts the `arn` from `{"user": {"arn": "..."}}`? | **`$.user.arn`**. |
| **03** | Why is CEF key extraction faster than regex parsing? | It performs **direct in-memory dictionary key lookups** with zero regex engine overhead. |
| **04** | Can you perform mathematical operations in AQL queries? | **Yes**, e.g., `(sourcebytes + destinationbytes) / 1024 AS KiloBytes`. |
| **05** | What parser library does QRadar use internally for JSONPath evaluation? | The high-speed **Jackson JSON Parser**. |
| **06** | What AQL function converts strings to lowercase for case-insensitive matching? | **`LOWER(string_property)`**. |

---

## 🧪 7. Hands-on Practice Lab 41: JSONPath & CEF Custom Property Extraction

### 🎯 Lab Objectives
1. Formulate JSONPath expressions for deeply nested AWS CloudTrail telemetry.
2. Inject synthetic JSON cloud events into the QRadar ingestion pipeline.
3. Validate field extraction for `Target_Bucket` and `IAM_Principal`.
4. Execute AQL queries utilizing derived mathematical properties.

---

### 📋 Step-by-Step Practical Lab Procedures

#### Step 1: Validate JSONPath Extraction Logic via Python
```bash
python3 -c "
import json

cloud_payload = '''{
  \"eventVersion\": \"1.08\",
  \"userIdentity\": {
    \"type\": \"IAMUser\",
    \"arn\": \"arn:aws:iam::123456789012:user/secadmin\"
  },
  \"eventSource\": \"s3.amazonaws.com\",
  \"eventName\": \"DeleteBucket\",
  \"requestParameters\": {
    \"bucketName\": \"customer-pii-vault\"
  }
}'''

data = json.loads(cloud_payload)
arn = data['userIdentity']['arn']
bucket = data['requestParameters']['bucketName']

print('=' * 50)
print('🚀 JSONPATH EXTRACTION TEST')
print('=' * 50)
print(f'Extracted ARN:    {arn}')
print(f'Extracted Bucket: {bucket}')
print('=' * 50)
"
```

---

#### Step 2: Inject Synthetic CloudTrail JSON Event
```bash
logger -n 127.0.0.1 -P 514 -t "AWS_CloudTrail" '{"eventVersion": "1.08", "userIdentity": {"type": "IAMUser", "arn": "arn:aws:iam::123456789012:user/secadmin"}, "eventSource": "s3.amazonaws.com", "eventName": "DeleteBucket", "requestParameters": {"bucketName": "customer-pii-vault"}}'
```

---

#### Step 3: Run AQL Query on Extracted Cloud Properties
```bash
/opt/qradar/bin/arielsql -p 32011 -q "
SELECT 
    DATEFORMAT(starttime, 'HH:mm:ss') AS Time, 
    sourceip, 
    qidname(qid) AS EventName, 
    \"AWS_IAM_ARN\" AS UserARN, 
    \"AWS_Target_Bucket\" AS TargetBucket 
FROM events 
WHERE UTF8(payload) ILIKE '%customer-pii-vault%' 
LAST 15 MINUTES;
"
```
