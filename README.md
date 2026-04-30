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

