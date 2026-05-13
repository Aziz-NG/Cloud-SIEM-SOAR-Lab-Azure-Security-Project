# ☁️ Cloud SIEM & SOAR Lab – Azure Security Project

> Hands-on implementation of a cloud-native SIEM & SOAR environment using Microsoft Sentinel, Entra ID, and Azure Logic Apps to simulate real-world SOC operations, identity threat detection, and automated incident response.

![Azure](https://img.shields.io/badge/Cloud-Microsoft%20Azure-blue)
![SIEM](https://img.shields.io/badge/Security-SIEM%20%7C%20SOAR-red)
![Sentinel](https://img.shields.io/badge/Platform-Microsoft%20Sentinel-green)
![Frameworks](https://img.shields.io/badge/Compliance-NIST%20%7C%20ISO%2027001%20%7C%20SOC2-orange)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)

---

# 📌 Problem Summary

Modern cloud environments face increasing exposure to **identity-based attacks** such as brute force attempts, password spraying, and suspicious authentication activity.

In many organizations, security teams struggle with:

* Detecting identity attacks in real time
* Correlating authentication anomalies across distributed logs
* Responding quickly to compromised accounts
* Reducing attacker dwell time
* Maintaining centralized visibility into authentication events

This project addresses these challenges by implementing a **cloud-native SIEM & SOAR platform using Microsoft Sentinel integrated with Entra ID telemetry** to provide centralized monitoring, detection engineering, and automated response capabilities.

---

# 🎯 Objectives

* Centralize authentication logging and monitoring
* Detect identity-based attacks using KQL analytics rules
* Automate incident response using Logic Apps
* Reduce Mean Time to Detect (MTTD)
* Reduce Mean Time to Respond (MTTR)
* Simulate real-world SOC detection and response workflows
* Map detections to the MITRE ATT&CK framework
* Align monitoring controls with NIST, ISO 27001, and SOC 2

---

# 🏛️ Architecture Overview

This environment integrates Azure-native security services to simulate enterprise SOC operations.

* **Entra ID (Azure AD)** — identity telemetry source
* **Azure Monitor** — centralized log ingestion
* **Log Analytics Workspace** — security event storage and querying
* **Microsoft Sentinel** — SIEM platform and incident correlation
* **Logic Apps** — SOAR automation and incident response
* **Azure Workbooks** — dashboards and visualization

---

## 🔄 Security Workflow

```text
Entra ID (Azure AD)
        |
        v
Azure Monitor + Diagnostic Settings
        |
        v
Log Analytics Workspace
        |
        v
Microsoft Sentinel
├── Analytics Rules
├── Incidents
├── Workbooks
└── Automation Rules
        |
        v
Logic Apps (SOAR Playbooks)
```

---

# 🛠️ Technologies & Skills Demonstrated

| Technology / Skill | Purpose |
|---|---|
| Microsoft Azure | Cloud infrastructure |
| Microsoft Sentinel | SIEM & threat detection |
| Entra ID (Azure AD) | Identity telemetry source |
| Azure Monitor | Log ingestion |
| Kusto Query Language (KQL) | Detection engineering |
| Logic Apps | SOAR automation |
| Azure Workbooks | SOC dashboards |
| STRIDE Threat Modeling | Security design analysis |
| MITRE ATT&CK Mapping | Threat classification |
| Attack Simulation | Detection validation |

---

# 🔍 Detection Rules & MITRE ATT&CK Mapping

## 🔹 Brute Force / Password Spraying

### 🧪 Detection Logic

```kusto
SigninLogs
| where ResultType != 0
| summarize FailedAttempts = count()
    by IPAddress, UserPrincipalName
| where FailedAttempts > 5
```

### 🎯 MITRE ATT&CK Mapping

| Technique | ID |
|---|---|
| Brute Force | T1110 |
| Password Spraying | T1110.003 |

### 🛡️ Security Benefit

Detects:

* High-frequency authentication failures
* Password spraying attacks
* Credential stuffing attempts

<img width="1880" height="860" alt="Password Spray Detection" src="YOUR_SCREENSHOT_HERE" />

---

# 🌍 Impossible Travel Detection

### 🧪 Detection Logic

```kusto
SigninLogs
| summarize Locations = make_set(Location)
    by UserPrincipalName, bin(TimeGenerated, 1h)
| where array_length(Locations) > 1
```

### 🎯 MITRE ATT&CK Mapping

| Technique | ID |
|---|---|
| Valid Accounts | T1078 |
| Proxy | T1090 |

### 🛡️ Security Benefit

Identifies suspicious authentication behavior where a user account authenticates from multiple geographically distant locations within an impossible timeframe.

This detection helps identify:

* Potential credential compromise
* Session hijacking
* VPN/proxy abuse
* Suspicious cross-region authentication activity

<img width="1880" height="860" alt="Impossible Travel Detection" src="YOUR_SCREENSHOT_HERE" />

---

# 🎯 MITRE ATT&CK Mapping

This project aligns detection logic and incident response workflows with the MITRE ATT&CK framework to classify adversary behavior and improve threat visibility.

| MITRE Technique | ID | Detection Coverage |
|---|---|---|
| Brute Force | T1110 | Repeated authentication failures |
| Password Spraying | T1110.003 | Multiple failed logins across users |
| Valid Accounts | T1078 | Impossible travel authentication |
| Proxy | T1090 | Geolocation anomaly detection |
| Account Discovery | T1087 | Authentication enumeration visibility |

### 🛡️ Security Benefit

Provides standardized threat classification aligned with enterprise SOC methodologies and modern detection engineering practices.

<img width="1880" height="860" alt="MITRE ATTACK Mapping" src="YOUR_SCREENSHOT_HERE" />

---

# ⚡ SOAR Automation – Playbooks

## 🔔 Playbook 1 – Incident Notification

### Trigger

* Microsoft Sentinel incident creation

### Actions

* Send email notification
* Send Microsoft Teams alert
* Include:
  * Username
  * Source IP
  * Alert severity
  * Incident title

### 🛡️ Security Benefit

Accelerates analyst awareness and improves incident triage efficiency.


---

## 🚫 Playbook 2 – Account Containment

### Trigger

* High-severity Sentinel incidents

### Actions

* Disable compromised Entra ID account
* Revoke active sessions
* Trigger automated remediation workflow

### 🛡️ Security Benefit

Reduces attacker dwell time by automating identity containment procedures.


---

# 🛡️ Threat Modeling

## 🎯 Primary Threat Scenario

Credential compromise targeting cloud identities and authentication services.

---

## 🔍 STRIDE Analysis

| Category | Example |
|---|---|
| Spoofing | Stolen credentials |
| Tampering | Log manipulation |
| Repudiation | Denying malicious actions |
| Information Disclosure | Unauthorized access |
| Denial of Service | Authentication flooding |
| Elevation of Privilege | Privilege escalation |

---

## 🌳 Attack Tree Analysis

Threat scenarios modeled include:

* Credential theft
* Detection evasion
* Persistence mechanisms
* Unauthorized privilege escalation

### 🛡️ Security Benefit

Helps identify attack paths and validate defensive coverage across the detection and response lifecycle.


---

# 🎯 Attack Simulation & Validation

Controlled attack simulations were performed to validate Sentinel detections and SOAR workflows.

| Scenario | Action | Result |
|---|---|---|
| Password Spraying | Multiple failed login attempts | Sentinel incident triggered |
| Impossible Travel | Login attempts from multiple regions | Alert generated successfully |
| SOAR Automation | High-severity incident simulation | Account disabled automatically |

---

## 🧪 Validation Results

### Password Spray Simulation

* Simulated repeated failed logins across multiple accounts
* Detection triggered successfully within Sentinel

### Impossible Travel Simulation

* Simulated authentication from geographically distant VPN endpoints
* Impossible travel alert generated and validated

### SOAR Automation Validation

* High-severity incident triggered automated remediation
* Account disabled and active sessions revoked

### 🛡️ Security Benefit

Confirms operational readiness of detection and response workflows.


---

# 📊 Visualization & Monitoring

Custom Azure Workbooks dashboards provide centralized SOC visibility and operational monitoring.

### Dashboard Components

* Authentication activity by country
* Failed vs successful sign-ins
* Alert severity distribution
* Authentication trend analysis
* SOC operational metrics
* Top targeted accounts

### 🛡️ Security Benefit

Improves threat visibility and supports real-time security monitoring across authentication events.


---

# 📈 Metrics & Outcomes

This project demonstrates measurable improvements in security visibility, detection speed, and incident response efficiency.

| Metric | Before | After Implementation |
|---|---|---|
| Mean Time to Detect (MTTD) | Manual / delayed | Near real-time |
| Mean Time to Respond (MTTR) | Hours | Minutes (automated) |
| Visibility Coverage | Partial | Centralized |
| Response Consistency | Variable | Standardized |

---

## ✅ Outcome Summary

* ✅ Reduced detection latency through centralized monitoring
* ✅ Improved incident response using SOAR automation
* ✅ Increased visibility into authentication activity
* ✅ Standardized SOC operational procedures
* ✅ Reduced attacker dwell time

---

# 🏛️ Governance, Risk & Compliance (GRC)

This project aligns with enterprise security frameworks commonly used in modern cloud environments.

---

## 📋 NIST Cybersecurity Framework (CSF)

| Function | Requirement | Implementation |
|---|---|---|
| DE.CM | Continuous Monitoring | Sentinel analytics + Entra ID logs |
| DE.AE | Detection of Anomalies | KQL-based detections |
| RS.MI | Incident Mitigation | Logic Apps remediation workflows |

---

## 📋 ISO/IEC 27001 Alignment

| Control | Requirement | Implementation |
|---|---|---|
| A.12.4 | Logging & Monitoring | Centralized Sentinel logging |
| A.16 | Incident Management | Automated incident response workflows |

---

## 📋 SOC 2 Alignment

| Criteria | Requirement | Implementation |
|---|---|---|
| CC7 | System Monitoring | Continuous authentication monitoring |
| CC7.2 | Incident Detection | Sentinel incident analytics |
| CC7.3 | Response Procedures | Automated SOAR playbooks |

---

# 📂 Repository Structure

```text
.
├── README.md
├── screenshots/
├── diagrams/
├── kql/
│   ├── brute_force_detection.kql
│   └── impossible_travel.kql
├── playbooks/
├── workbooks/
└── architecture/
```

---

# 🧠 Key Security Outcomes

This implementation demonstrates:

- Identity-first security architecture (Zero Trust model)
- Detection engineering using KQL (Detection-as-Code)
- SOC automation using Logic Apps (SOAR engineering)
- MITRE ATT&CK-aligned detection strategy
- Real-world SOC workflow simulation
- Automated containment of compromised identities

---

# 🚀 Key Takeaways

This project reflects how modern Microsoft security teams operate:

- Shift from reactive monitoring → proactive detection engineering  
- Shift from manual SOC operations → automated response systems  
- Identity as the primary security perimeter in cloud environments  
- Integration of SIEM + SOAR for closed-loop security operations  
- Operational use of MITRE ATT&CK in detection design 

---

# 📌 Conclusion

This project showcases a production-aligned **Microsoft Sentinel SOC engineering implementation** capable of:

- Detecting identity-based threats in real time  
- Correlating security telemetry across Azure services  
- Automating incident response using SOAR playbooks  
- Reducing SOC response time and operational overhead  
- Aligning with enterprise security frameworks (NIST, ISO, SOC 2)  

It represents a complete **cloud SOC simulation built on Microsoft security architecture principles**.

---

