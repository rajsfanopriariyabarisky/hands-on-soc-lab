# 🛡️ Hands-On SOC Lab: Wazuh SIEM Deployment & SSH Brute Force Detection

## 📌 Overview
This repository documents the implementation of a functional SIEM/XDR Home Lab built with **Wazuh**. The lab simulates defensive SOC operations, focusing on telemetry ingestion, threat detection, log analysis, and mapping security alerts to the **MITRE ATT&CK** framework.

---

## 🏗️ Architecture & Infrastructure
* **SIEM Core (Manager / Indexer / Dashboard):** Ubuntu Server VM (`192.168.52.129`)
* **Endpoint (Wazuh Agent):** Windows Host (`192.168.52.1`)
* **Tooling & Protocol:** Wazuh v4.9.2, OpenSSL, Syslog, PowerShell, SSH

---

## 📸 Endpoint Status & Infrastructure Verification

### Active Windows Agent Monitoring
<img width="1919" height="1059" alt="ss soc 4" src="https://github.com/user-attachments/assets/c1ffb2c6-6b45-4758-ae41-6e39c71cd2d3" />


---

## ⚔️ Attack Simulation & Detection Workflow

1. **Telemetry Collection:**
   Integrated Windows host as an active Wazuh Agent to forward telemetry alongside Linux authentication logs (`/var/log/auth.log`).

2. **Attack Simulation:**
   Executed multiple failed SSH authentication attempts from the Windows host targeting the Linux server to simulate a brute force pattern.

3. **Alert Triggering:**
   Wazuh Indexer processed the ingested logs and matched them against rule signatures, elevating the alert to **Level 10 (High Risk)**.
   * **Rule ID:** `5712`
   * **Description:** `sshd: brute force trying to get access to the system.`

### 🚨 Real-time Security Event Ingestion (Level 10 Alert Triggered)
<img width="959" height="533" alt="ss soc 1" src="https://github.com/user-attachments/assets/0faa4541-d3da-4513-a1c9-08508ea3f301" />


4. **MITRE ATT&CK Mapping:**
   * **Tactics:** Credential Access
   * **Technique:** Brute Force ([T1110](https://attack.mitre.org/techniques/T1110/)) / Password Guessing ([T1110.001](https://attack.mitre.org/techniques/T1110/001/))

---

## 🔍 Root Cause Analysis (JSON Log Forensic)

### 🔬 Raw Telemetry Forensic View
<img width="1909" height="1059" alt="ss soc 2" src="https://github.com/user-attachments/assets/dad99ff6-bc1c-48c4-90d0-7fda36ababa6" />


Raw log telemetry extracted from the Wazuh Manager:

```json
{
  "agent": {
    "id": "000",
    "name": "soc-server"
  },
  "rule": {
    "id": "5712",
    "level": 10,
    "description": "sshd: brute force trying to get access to the system."
  },
  "previous_output": "Failed password for invalid user hacker from 192.168.52.1 port 45672 ssh2"
}

📊 Visual Threat Dashboard & MITRE ATT&CK Overview
SOC Threat Hunting Dashboard
<img width="1919" height="1066" alt="ss 3 soc" src="https://github.com/user-attachments/assets/094e3fd7-21de-44f2-9f6c-e4db551dd069" />


🚀 Key Takeaways
Established end-to-end visibility across heterogeneous systems (Linux Server & Windows Endpoint).

Validated real-time alert generation from raw authentication failures to visual SOC dashboards.

Gained practical experience in threat hunting, log decoding, and incident evidence documentation.
