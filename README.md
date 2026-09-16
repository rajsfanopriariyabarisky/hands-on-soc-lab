<img width="1919" height="1059" alt="ss soc 4" src="https://github.com/user-attachments/assets/f7938002-cf6c-4349-8331-a3c9b11d9ced" /># 🛡️ Hands-On SOC Lab: Wazuh SIEM Deployment & SSH Brute Force Detection


## 📌 Overview
This repository documents the implementation of a functional SIEM/XDR Home Lab built with **Wazuh**. The lab simulates defensive SOC operations, focusing on telemetry ingestion, threat detection, log analysis, and mapping security alerts to the **MITRE ATT&CK** framework.

---

## 🏗️ Architecture & Infrastructure
* **SIEM Core (Manager / Indexer / Dashboard):** Ubuntu Server VM (`192.168.52.129`)
* **Endpoint (Wazuh Agent):** Windows Host (`192.168.52.1`)
* **Tooling & Protocol:** Wazuh v4.9.2, OpenSSL, Syslog, PowerShell, SSH

---

## ⚔️ Attack Simulation & Detection Workflow
<img width="1919" height="1059" alt="ss soc 4" src="https://github.com/user-attachments/assets/f2769189-095d-4dd3-8d84-5670e6566024" />
<img width="1909" height="1059" alt="ss soc 2" src="https://github.com/user-attachments/assets/7ef58be3-50c2-45ff-bc57-b7d3ed2f1588" />
<img width="959" height="533" alt="ss soc 1" src="https://github.com/user-attachments/assets/0df1f923-ebe1-464a-87f3-0cb0d0f44a48" />
<img width="1919" height="1066" alt="ss 3 soc" src="https://github.com/user-attachments/assets/ad8dace4-0377-4e0b-a5ec-266897c5748f" />


1. **Telemetry Collection:**
   Integrated Windows host as an active Wazuh Agent to forward telemetry alongside Linux authentication logs (`/var/log/auth.log`).

2. **Attack Simulation:**
   Executed multiple failed SSH authentication attempts from the Windows host targeting the Linux server to simulate a brute force pattern.

3. **Alert Triggering:**
   Wazuh Indexer processed the ingested logs and matched them against rule signatures, elevating the alert to **Level 10 (High Risk)**.
   * **Rule ID:** `5712`
   * **Description:** `sshd: brute force trying to get access to the system.`

4. **MITRE ATT&CK Mapping:**
   * **Tactics:** Credential Access
   * **Technique:** Brute Force ([T1110](https://attack.mitre.org/techniques/T1110/)) / Password Guessing ([T1110.001](https://attack.mitre.org/techniques/T1110/001/))

---

## 🔍 Root Cause Analysis (JSON Log Forensic)
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
