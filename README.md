# OT/ICS Security Laboratory: Modbus TCP Protocol Analysis

## Overview
This laboratory environment demonstrates the deployment, operational integration, and protocol analysis of an Operational Technology (OT) / Industrial Control System (ICS) architecture using containerized services. The environment models a modern Supervisory Control and Data Acquisition (SCADA) network polling telemetry from a Programmable Logic Controller (PLC) using unencrypted Modbus TCP.

## Architecture & Technology Stack
* **Host Environment:** Kali Linux
* **Containerization:** Docker
* **PLC Runtime:** OpenPLC Runtime (Port 502)
* **SCADA/HMI:** FUXA SCADA (Port 1881)
* **Protocol & Network Analysis:** Modbus TCP / Wireshark
```
+--------------------+               +-----------------------+
|    FUXA SCADA      |  Modbus TCP   |    OpenPLC Runtime    |
|   (HMI Dashboard)  | ------------> |    (PLC Engine)       |
| 172.17.0.x / 1881  |   Port 502    |  172.17.0.2 / Port 502|
+--------------------+               +-----------------------+
```
## Key Lab Achievements
1. **Containerized OT Deployment:** Configured Dockerized OpenPLC Runtime and FUXA SCADA instances within an isolated virtual network bridge.
2. **SCADA-PLC Integration:** Configured Modbus TCP communications, registering memory tags (`Temp_Sensor` on Holding Register `0`) mapped to live dashboard visual widgets (Gauge UI).
3. **Traffic Analysis & Telemetry Baselining:** Captured and analyzed unencrypted industrial control traffic via Wireshark on TCP Port 502, verifying standard Function Code 3 (`FC03 - Read Holding Registers`) request/response polling loops.

## Protocol Findings & Security Analysis
* **Protocol Vulnerability:** Modbus TCP operates as a legacy protocol lacking native cryptographic protection, packet signing, or authentication mechanisms.
* **Risk Context:** Any node connected to the control network with logical access to TCP port 502 can inspect or transmit industrial function commands directly to the controller runtime.
* **Defensive Controls:** 
  * Network micro-segmentation adhering to the Purdue Model / IEC 62443 standards.
  * Deep Packet Inspection (DPI) industrial firewalls filtering specific Modbus function codes.
  * Deployment of Modbus Security (MBAPS) using TLS encapsulation on TCP port 802.

## Repository Structure
* `/captures`: Wireshark packet captures (`.pcapng`) demonstrating Modbus TCP baseline polling loops.
* `/docs`: Architecture layouts and visual evidence.
