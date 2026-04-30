# Wazuh SOC Lab Implementation

## Project Overview
This project demonstrates the end-to-end deployment of a Security Operations Center (SOC) using the Wazuh platform.

The lab simulates a real-world enterprise security environment, including log collection, threat detection, attack simulation, and automated response.

---

## Key Capabilities

-  Centralized Log Monitoring  
-  Endpoint Detection & Response (EDR)  
-  File Integrity Monitoring (FIM)  
-  Vulnerability Detection  
-  Security Configuration Assessment (SCA)  
-  Rootcheck (Malware/Rootkit Detection)  
-  Custom Detection Rules  
-  Intrusion Prevention (Active Response / IPS)  

---

##  Lab Architecture

### Virtual Environment Setup

| System       | Role                                      |
|-------------|-------------------------------------------|
| Ubuntu      | Wazuh Manager + Indexer + Dashboard       |
| Kali Linux  | Attacker + Agent                          |
| Windows 10  | Agent + Sysmon                            |

---

###  Network Configuration

| Machine      | IP Address        | Role                                      |
|-------------|------------------|-------------------------------------------|
| Ubuntu      | 192.168.56.104   | Wazuh Manager + Indexer + Dashboard       |
| Kali Linux  | 192.168.56.105   | Attacker + Agent                          |
| Windows 10  | 192.168.56.106   | Agent + Sysmon                            |

---

##  Data Flow Architecture

###  Log Processing Workflow


---

##  Architecture Explanation

### 1️ Agent
The Wazuh agent is installed on endpoint systems such as Windows and Kali Linux.

- Collects system, security, and application logs  
- Monitors endpoint activities in real time  
- Forwards collected data to the Wazuh Manager  

Acts as a **distributed data collection sensor**

---

### 2️ Manager
The Wazuh Manager serves as the core processing unit of the SOC.

- Receives and analyzes logs from agents  
- Applies predefined and custom detection rules  
- Correlates events and generates security alerts
  
  Functions as the **central analysis engine (SOC brain)**

---

### 3️ Indexer
The Wazuh Indexer is responsible for data storage and retrieval.

- Stores processed logs and alerts securely  
- Enables high-speed search and querying  
- Supports efficient data indexing for large-scale environments  

Operates as a **high-performance log storage and search engine**

---

### 4️ Dashboard
The Wazuh Dashboard provides a graphical interface for monitoring and analysis.

- Displays alerts, logs, and security events  
- Offers visualization through charts and dashboards  
- Enables SOC analysts to investigate and respond to threats  

 Acts as the **visual monitoring and analysis interface**

 #  WEEK 1 – SOC Deployment

##  Objective
Deploy the Wazuh server infrastructure and register multiple agents for centralized monitoring.

---

##  Wazuh Installation (Ubuntu)

The Wazuh all-in-one installation script was used to deploy:
- Wazuh Manager  
- Wazuh Indexer  
- Wazuh Dashboard  

### Installation Steps

```bash
sudo apt update
sudo apt install curl -y
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

<img width="959" height="548" alt="wazhu" src="https://github.com/user-attachments/assets/adfd8a52-8e38-468f-8392-8fb736120731" />

---

##  Agent Deployment

###  Windows Agent

The Wazuh agent was successfully installed and started on the Windows system.

```bash
NET START wazuhSvc
```
<img width="940" height="353" alt="554064423-d984f194-7621-4536-bb0d-9bf893955a43" src="https://github.com/user-attachments/assets/2eac2d0e-5358-4997-b98e-85749299054a" />
Kali agent:

```
sudo apt install wazuh-agent -y
sudo systemctl start wazuh-agent
```
<img width="940" height="482" alt="554066756-03235140-8bf5-4ebe-9141-e94bcfd7b4f4" src="https://github.com/user-attachments/assets/a3ffb622-ad6a-4902-8336-86420e0f5e1d" />

<img width="940" height="198" alt="554069865-e2abb948-df21-4c32-9275-842c15f84164 (1)" src="https://github.com/user-attachments/assets/32e2a61b-a5b7-4700-8c79-7ceaaea45ba8" />

##  Sysmon Installation (Windows)

Sysmon (System Monitor) was deployed to enhance endpoint visibility and security monitoring on the Windows system.

### Key Features

- Process creation and execution monitoring  
- Network connection tracking  
- Registry modification tracking  

###  Sysmon Setup
<img width="533" height="205" alt="554067678-33b0fa3d-5d49-43f9-af6c-71883d4293b7" src="https://github.com/user-attachments/assets/b7025b2a-10fa-4f4e-ab20-c22a456e09d6" />

---

# WEEK 2 – Advanced Monitoring

### Monitored directory:
```
/var/www
```

### Configuration:
```
<directories realtime="yes" check_all="yes">/var/www</directories>
Tested by:
```
1.Creating file

2.Modifying file

3.Deleting file
<img width="1596" height="829" alt="WhatsApp Image 2026-04-30 at 8 41 28 AM" src="https://github.com/user-attachments/assets/eaa05911-7b6d-4571-9de8-4b082a72aa01" />

<img width="1600" height="814" alt="WhatsApp Image 2026-04-30 at 8 41 29 AM" src="https://github.com/user-attachments/assets/e0e45d92-7280-4162-8c58-5afead1e9854" />

Vulnerability Detector Enabled CVE scanning: enabled = yes → Turn CVE scanning ON

interval = 5m → Check every 5 minutes

run_on_start = yes → Scan immediately after restart

<img width="1600" height="367" alt="WhatsApp Image 2026-04-30 at 8 41 29 AM (1)" src="https://github.com/user-attachments/assets/584eb2b3-f4e2-479b-bc40-6e407c4a08b0" />

---
# WEEK 3 – Intrusion Prevention System (IPS)
### Aim:
Simulate an SSH brute force attack and automatically block attacker IP using Wazuh Active Response.

SSH Setup (Ubuntu)

```
sudo apt install openssh-server -y
sudo systemctl start ssh
```
<img width="1599" height="902" alt="WhatsApp Image 2026-04-30 at 9 05 43 AM" src="https://github.com/user-attachments/assets/fb99228f-c324-4f3b-b3e5-5a452625d53e" />


# Brute Force Attack (Kali)
```
hydra -l sakshi -P /usr/share/wordlists/rockyou.txt -t 4 ssh://192.168.56.104
```
<img width="1600" height="433" alt="WhatsApp Image 2026-04-30 at 9 05 43 AM (1)" src="https://github.com/user-attachments/assets/a94d4dbb-4983-4ee1-a872-1d4cbeffc3cb" />


## Detection in Wazuh
### Search query used:
```
rule.id:5710 OR rule.id:5712 OR rule.id:5716
```
<img width="1600" height="897" alt="WhatsApp Image 2026-04-30 at 9 05 44 AM" src="https://github.com/user-attachments/assets/f256ec52-5ed7-454d-ab4c-f42f15cf7d9c" />

# Active Response Configuration:

Added inside ossec.conf:
```
<active-response>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5710,5712,5716</rules_id>
  <timeout>600</timeout>
</active-response>
```

# Restarted service:
```
sudo systemctl restart wazuh-manager
```
<img width="1600" height="689" alt="WhatsApp Image 2026-04-30 at 9 05 44 AM (1)" src="https://github.com/user-attachments/assets/bda44511-0a1d-4eff-956a-b7a9ad7ccad7" />

## Automatic IP Blocking:
After attack:

Hydra stopped

SSH connection denied

### Firewall rule inserted automatically Verification:

```
ssh sakshi@192.168.56.104
```
<img width="1600" height="469" alt="WhatsApp Image 2026-04-30 at 9 05 45 AM" src="https://github.com/user-attachments/assets/a8404aa6-40ae-4c77-bbb9-a640d524d765" />


# Alerts summary

<img width="1600" height="659" alt="WhatsApp Image 2026-04-30 at 9 05 45 AM (1)" src="https://github.com/user-attachments/assets/e599262c-4447-45de-be4f-f68bfd06753c" />


## 📊 Alerts Summary

| Phase      | Action Performed                          | Outcome                                      |
|-----------|-------------------------------------------|----------------------------------------------|
| Attack     | SSH brute force using Hydra              | Multiple failed login attempts generated     |
| Detection  | Wazuh rules 5710 / 5712 triggered        | Security alerts created in dashboard         |
| Response   | firewall-drop active response executed   | Attacker IP automatically blocked            |
| Validation | SSH connection retried from Kali         | Access denied / Connection blocked           |

---

##  Technologies Used

- Wazuh SIEM  
- Ubuntu Linux  
- Kali Linux  
- Windows 10  
- Sysmon  
- Hydra  
- VirtualBox  
- Linux Firewall (iptables)  

##  Skills Demonstrated

This project showcases a comprehensive set of practical cybersecurity and SOC-related skills, including:

- **SOC Architecture Deployment** – Designed and implemented a fully functional Security Operations Center (SOC) lab environment using Wazuh  
- **Centralized Log Monitoring & Analysis** – Collected, correlated, and analyzed logs from multiple endpoints in real time  
- **Attack Simulation & Adversary Emulation** – Performed controlled brute-force attacks to replicate real-world threat scenarios  
- **Detection Rule Validation** – Utilized and verified Wazuh detection rules to accurately identify malicious activities  
- **Intrusion Prevention Configuration (IPS)** – Implemented automated response mechanisms to block attacker activity  
- **Firewall Automation & Response Handling** – Configured dynamic firewall rules using Active Response for immediate threat mitigation  
- **Real-Time Incident Detection & Validation** – Monitored alerts and validated security incidents through live testing  
- **System Troubleshooting & Debugging** – Diagnosed and resolved issues across agents, services, and configurations  

---

## 🏁 Final Conclusion

This project successfully demonstrates a complete SOC lifecycle:

Monitoring → Detection → Alerting → Automated Response → Validation

The lab replicates enterprise-level security monitoring and automated threat mitigation using Wazuh. It highlights practical implementation of defensive security engineering and real-world SOC operations.

# WEEK 4 – Threat Simulation & MITRE ATT&CK Visualization

## Objective

Simulate adversary behavior and observe how Wazuh SIEM detects the activity and maps it to the MITRE ATT&CK framework.
This exercise demonstrates how SOC analysts monitor suspicious activity and analyze attack stages through a kill chain visualization.

## Threat Simulation

A suspicious command was executed on the Windows endpoint to generate activity monitored by Sysmon and analyzed by Wazuh.

Command executed on Windows PowerShell (Administrator):
```
powershell.exe -NoProfile -ExecutionPolicy Bypass -Command "Get-Process"
```

<img width="940" height="471" alt="image" src="https://github.com/user-attachments/assets/41fc4ebd-d91e-47ae-8dcd-407ca683b345" />
Purpose:

- Generate process creation events
- Allow Sysmon to log command execution
- Send telemetry to Wazuh SIEM

# Sysmon Log Verification
To confirm that Sysmon was generating endpoint logs, the following command was executed:

```
Get-WinEvent -LogName "Microsoft-Windows-Sysmon/Operational" -MaxEvents 5
Observed events included:
```
- Event ID 1 – Process Create
- Event ID 11 – File Create
These logs provide the telemetry required for threat detection in the SOC environment.

<img width="940" height="298" alt="image" src="https://github.com/user-attachments/assets/df71f85d-766b-46f4-912b-9133276c72ae" />

# Wazuh Agent Configuration
To enable Sysmon log collection, the Wazuh agent configuration file was modified.

## File edited:
```
C:\Program Files (x86)\ossec-agent\ossec.conf
Configuration added:
```
## Configuration added:
```
<localfile> <location>Microsoft-Windows-Sysmon/Operational</location> <log_format>eventchannel</log_format> </localfile>
```
<img width="913" height="433" alt="image" src="https://github.com/user-attachments/assets/e91eb0ad-6e53-4096-9347-81371e107f91" />

After updating the configuration, the Wazuh agent was restarted:
```
Restart-Service wazuh
```

## Detection in Wazuh:

# Detection in Wazuh
The suspicious activity generated alerts in the Wazuh Security Events dashboard.

Example alert detected:

| Field        | Value                                              |
|-------------|----------------------------------------------------|
| Rule ID     | 92213                                              |
| Alert Level | 15                                                 |
| Description | Executable file dropped in folder commonly used by malware |

This alert indicates that Wazuh successfully detected suspicious activity on the monitored Windows endpoint.

<img width="940" height="132" alt="image" src="https://github.com/user-attachments/assets/0d8baf97-fa2c-4bfb-b3cd-f1f7c8343139" />

# MITRE ATT&CK Mapping
The detected alert was automatically mapped to the MITRE ATT&CK framework, which helps security analysts understand attacker behavior and classify threats according to standardized adversary techniques.

| MITRE Field    | Value                |
|----------------|----------------------|
| Technique ID   | T1105                |
| Tactic         | Command and Control  |

# Technique Explanation
## T1105 – Ingress Tool Transfer

This technique represents a stage where attackers transfer tools or malicious payloads into a compromised system. These tools may be used to establish command-and-control communication, perform lateral movement, or execute further malicious activities within the target environment.

In this lab, the suspicious activity detected by Wazuh SIEM was mapped to this MITRE technique, allowing analysts to clearly understand the adversary behavior and visualize the attack stage within the cyber kill chain.

This mapping helps SOC teams quickly categorize threats and respond effectively to potential security incidents.

<img width="940" height="334" alt="image" src="https://github.com/user-attachments/assets/2efc408e-e16d-4fe4-92d5-456ddafd65bd" />

## 🔗 Kill Chain Visualization

The following workflow illustrates the end-to-end attack detection and analysis process within the SOC lab environment:

- Suspicious Command Execution
- ↓
- Sysmon Logs System Activity
- ↓
- Log Forwarding via Wazuh Agent
- ↓
- Wazuh Detection Rules Triggered
- ↓
- Alert Generation
- ↓
- MITRE ATT&CK Mapping
- ↓
- Visualization in Wazuh Dashboard


This workflow demonstrates how a SOC analyst can effectively trace adversary behavior across multiple stages of the cyber kill chain, from initial activity to detection and visualization.

---

## ✅ Result

The threat simulation successfully generated endpoint telemetry, which was captured by Sysmon and forwarded to the Wazuh SIEM for analysis.

Key outcomes include:

- Successful generation and collection of endpoint activity logs  
- Accurate detection of suspicious behavior by Wazuh  
- Alert generation based on predefined detection rules  
- Automatic mapping of alerts to MITRE ATT&CK techniques  
- Clear visualization of attack behavior within the SOC dashboard  

This validates the effectiveness of the monitoring and detection pipeline implemented in the lab.

---

## 🏁 Conclusion

This exercise demonstrates the practical integration of Sysmon with Wazuh SIEM to achieve enhanced endpoint visibility and advanced threat detection.

By correlating system-level telemetry with detection rules and mapping events to the MITRE ATT&CK framework, SOC analysts can:

- Identify and analyze malicious behavior in real time  
- Understand attacker techniques using standardized threat models  
- Improve incident response and threat investigation workflows  

Overall, this implementation reflects real-world SOC operations and highlights the importance of combining log monitoring, detection logic, and threat intelligence frameworks to build a robust and proactive security posture.








