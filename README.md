# AD Homelab Project

## Part 1 — Network Diagram

### Objective
Create a logical AD homelab diagram to understand network structure and telemetry flow.

### Skills Learned
* Network diagramming
* AD lab architecture
* Telemetry flow mapping

### Tools
* draw.io

### Steps

#### Ref 1: Network Diagram
<img src="diagram-part1.png" width="800">

## Lab Architecture Summary
Active Directory server and Windows 11 machine both generate telemetry via Sysmon and forward logs to Splunk using Splunk Universal Forwarder for centralized monitoring. Kali Linux serves as the attacker system connected to the same network for future attack simulation.
