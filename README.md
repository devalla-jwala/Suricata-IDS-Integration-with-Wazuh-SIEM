# Suricata-IDS-Integration-with-Wazuh-SIEM
his project demonstrates the integration of **Suricata IDS (Intrusion Detection System)** with **Wazuh SIEM** for real-time network threat detection, monitoring, and analysis.  A virtual SOC lab environment was created using **VirtualBox**,This project simulates a real-world SOC workflow involving IDS alert generation
# Suricata IDS Integration with Wazuh SIEM

## 📌 Project Overview

This project demonstrates the integration of **Suricata IDS (Intrusion Detection System)** with **Wazuh SIEM** for real-time network threat detection, monitoring, and analysis.

A virtual SOC lab environment was created using **VirtualBox**, where attack traffic generated from a Kali Linux machine was detected by Suricata running on an Ubuntu server. The generated alerts were forwarded to Wazuh for centralized monitoring, log analysis, and threat hunting.

This project simulates a real-world SOC workflow involving IDS alert generation, SIEM log collection, event correlation, and security monitoring.

---

# 🎯 Objectives

- Deploy and configure Suricata IDS
- Integrate Suricata with Wazuh SIEM
- Generate attack traffic using Kali Linux
- Monitor Suricata alerts in Wazuh
- Investigate security events through Threat Hunting
- Demonstrate real-time IDS-to-SIEM integration

---

# 🏗️ Lab Architecture

```text
+----------------+
| Kali Linux     |
| 10.203.144.30  |
+--------+-------+
         |
         | Nmap Scan
         |
         v

+----------------+
| Ubuntu Server  |
| 10.203.144.148 |
| Apache Server  |
| Suricata IDS   |
| Wazuh Agent    |
+--------+-------+
         |
         | eve.json Alerts
         |
         v

+----------------+
| Wazuh Server   |
| 10.203.144.122 |
+--------+-------+
         |
         v

+----------------+
| Wazuh Dashboard|
| Threat Hunting |
+----------------+
```

---

# 🖥️ Lab Environment

| Component | Role |
|------------|------------|
| Kali Linux | Attack Machine |
| Ubuntu 24.04 LTS | Victim Machine |
| Suricata IDS | Network Monitoring |
| Apache2 | Target Web Server |
| Wazuh Agent | Log Collection |
| Wazuh Server | SIEM Platform |
| VirtualBox | Virtualization |

---

# 🛠️ Tools & Technologies

- Wazuh SIEM 4.14.5
- Suricata IDS 7.0.3
- Ubuntu 24.04 LTS
- Kali Linux
- Apache2
- Nmap
- VirtualBox
- Linux
- JSON Logs

---

# 📥 Step 1: Install Suricata

Update packages:

```bash
sudo apt update
```

Install Suricata:

```bash
sudo apt install suricata -y
```

Verify installation:

```bash
suricata --build-info
```

---

# ✅ Step 2: Validate Configuration

Check Suricata configuration:

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml
```

Expected output:

```text
Configuration provided was successfully loaded.
Exiting.
```

---

# 🚀 Step 3: Start Suricata

Enable service:

```bash
sudo systemctl enable suricata
```

Start service:

```bash
sudo systemctl start suricata
```

Verify:

```bash
sudo systemctl status suricata
```

Expected:

```text
Active: active (running)
```

---

# 📂 Step 4: Verify Alert Logging

Monitor Suricata events:

```bash
sudo tail -f /var/log/suricata/eve.json
```

Alert events appear as:

```json
{
  "event_type":"alert",
  "alert":{
    "signature":"ET SCAN Possible Nmap User-Agent Observed"
  }
}
```

---

# 🔗 Step 5: Configure Wazuh Integration

Edit Wazuh Agent configuration:

```bash
sudo nano /var/ossec/etc/ossec.conf
```

Add the following:

```xml
<!-- Suricata IDS Logs -->

<localfile>
  <log_format>json</log_format>
  <location>/var/log/suricata/eve.json</location>
</localfile>
```

Save and exit.

Restart Wazuh Agent:

```bash
sudo systemctl restart wazuh-agent
```

Verify:

```bash
sudo systemctl status wazuh-agent
```

---

# 🌐 Step 6: Deploy Apache Web Server

Install Apache:

```bash
sudo apt install apache2 -y
```

Verify:

```bash
sudo systemctl status apache2
```

Confirm web access:

```bash
http://10.203.144.148
```

---

# 🎯 Step 7: Simulate an Attack

From Kali Linux:

```bash
nmap -sS -sV 10.203.144.148
```

Example Output:

```text
PORT   STATE SERVICE VERSION

80/tcp open  http

Apache httpd 2.4.58
```

This reconnaissance activity generates IDS alerts.

---

# 🚨 Detection Results

Suricata successfully detected the scan and generated alerts.

Example Signatures:

```text
ET SCAN Possible Nmap User-Agent Observed

SURICATA HTTP Request

SURICATA STREAM Packet
```

---

# 📊 Wazuh Detection

Navigate to:

```text
Threat Hunting
```

Apply filter:

```text
rule.id:86601
```

Observed Alerts:

```text
Rule ID: 86601

Level: 3

Group: ids, suricata
```

Example Alert:

```text
Suricata Alert - ET SCAN Possible Nmap User-Agent Observed
```

---

# 🔍 Investigation

## Source IP

```text
10.203.144.30
```

Kali Linux (Attacker)

---

## Destination IP

```text
10.203.144.148
```

Ubuntu Server (Target)

---

## Attack Type

```text
Reconnaissance
```

---

## Detection Technique

```text
Signature-Based Detection
```

---

# 📈 Security Workflow

```text
Nmap Scan
      ↓
Suricata Detects Activity
      ↓
Alert Written to eve.json
      ↓
Wazuh Agent Reads Log
      ↓
Wazuh Manager Processes Event
      ↓
Rule Matching (86601)
      ↓
Threat Hunting Dashboard
      ↓
SOC Investigation
```

---

# 📷 Screenshots

Create a folder named:

```text
screenshots
```

Add:

```text
screenshots/
│
├── architecture.png
├── suricata-status.png
├── suricata-config-test.png
├── eve-json-alerts.png
├── wazuh-rule-86601.png
├── threat-hunting-alerts.png
├── nmap-scan.png
└── wazuh-agent-active.png
```

Reference them:

```markdown
## Suricata Running

![Suricata](screenshots/suricata-status.png)

## Nmap Scan

![Nmap](screenshots/nmap-scan.png)

## Suricata Alerts

![eve](screenshots/eve-json-alerts.png)

## Wazuh Detection

![alerts](screenshots/wazuh-rule-86601.png)

## Threat Hunting Dashboard

![dashboard](screenshots/threat-hunting-alerts.png)
```

---

# 🎓 Skills Demonstrated

- SIEM Monitoring
- Threat Hunting
- IDS Deployment
- Security Event Analysis
- Incident Investigation
- Log Analysis
- Linux Administration
- Apache Administration
- Network Monitoring
- Nmap Detection
- Wazuh Administration
- Suricata Configuration
- Security Operations Center (SOC)

---

# 🏆 Project Outcome

Successfully integrated **Suricata IDS** with **Wazuh SIEM** to provide centralized visibility into network security events.

The project demonstrated:

- Real-time network threat detection
- IDS alert generation
- SIEM log ingestion
- Security event correlation
- Threat hunting workflows
- Reconnaissance attack detection using Nmap

This implementation reflects a practical SOC use case where IDS alerts are monitored and investigated through a centralized SIEM platform.

---

# 📄 Resume Description

**Suricata IDS Integration with Wazuh SIEM**

Designed and deployed a virtual SOC lab integrating Suricata IDS with Wazuh SIEM for real-time network threat monitoring. Configured Suricata to detect reconnaissance activities and forwarded JSON-based alerts to Wazuh for centralized analysis. Simulated attacks using Kali Linux Nmap scans, investigated alerts through the Wazuh Threat Hunting dashboard, and demonstrated IDS-to-SIEM event correlation, log analysis, and security monitoring workflows.
