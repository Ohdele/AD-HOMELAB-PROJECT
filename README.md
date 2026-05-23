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

---


# Part 2 — Virtual Machine Deployment

## Objective
Deploy and configure the virtual machines required for the AD homelab environment.

## Skills Learned
* Virtual machine deployment
* VM resource allocation
* ISO verification using SHA256 checksum
* Basic Active Directory infrastructure understanding

## Tools
* Oracle VirtualBox

## Steps

### Ref 1: Virtual Machine Installation
<img src="vbox-part2-overview.png" width="800">

VirtualBox manager showing all four VMs (Win10, Kali, Win Server 2022, Ubuntu Server) with Ubuntu Server 24.04 LTS configuration details displayed, used as the primary infrastructure node.

## Summary
ISO files were verified using SHA256 to ensure integrity before installation. VM resources were assigned based on role, with Splunk allocated higher capacity for log ingestion and search performance. Final setup provides a stable AD lab environment for telemetry and attack simulation.
