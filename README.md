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

### 🔹 Virtual Environment Setup

| System       | Role                                      |
|-------------|-------------------------------------------|
| Ubuntu      | Wazuh Manager + Indexer + Dashboard       |
| Kali Linux  | Attacker + Agent                          |
| Windows 10  | Agent + Sysmon                            |

---

### 🌐 Network Configuration

| Machine      | IP Address        | Role                                      |
|-------------|------------------|-------------------------------------------|
| Ubuntu      | 192.168.56.104   | Wazuh Manager + Indexer + Dashboard       |
| Kali Linux  | 192.168.56.105   | Attacker + Agent                          |
| Windows 10  | 192.168.56.106   | Agent + Sysmon                            |

---

## 🔄 Data Flow Architecture
