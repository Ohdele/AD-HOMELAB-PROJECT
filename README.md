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

---


# Part 3 — Splunk Data Ingestion & Verification

## Objective
Install & configure Sysmon and Splunk on Windows 10 and Windows Server so they can start collecting telemetry and send logs to Splunk server for monitoring and analysis.

## Skills Learned
* Windows event logging and Sysmon telemetry generation
* Splunk Universal Forwarder deployment and configuration
* Log ingestion and indexing in Splunk (`inputs.conf`, custom index setup)
* NAT networking and static IP configuration in a lab environment
* Multi-host SIEM data pipeline validation (Win10 + Windows Server → Forwarder → Splunk Indexer)

## Tools
* Splunk Enterprise (SIEM) / Universal Forwarder
* Sysmon
* Windows 10/Server VMs
* Ubuntu Server
* VirtualBox / NAT Network
* PowerShell / Windows Event Viewer

## Steps

### Ref 1: Splunk Log Ingestion Confirmation
<img src="part3-screenshots/01_Splunk_Ingestion.png" width="800">

Splunk search results showing 7,196 events from 2 hosts and values are target-pc and WIN-NEB6ORBJ7S7, confirming successful centralized log ingestion from multiple endpoints.

### Ref 2: Sysmon Log Source Search
<img src="part3-screenshots/02_Splunk_Source.png" width="800">

Splunk search showing source(4) and source type(2). My inputs.config file specifically mentioned Security, Application, System and Sysmon data under Values.

## Summary
Splunk confirmed successful ingestion of telemetry from 2 hosts, with key log sources (Sysmon, Security, Application, System) from inputs.config. No anomalies or security incidents identified at this stage; logs represent baseline system activity.

---


# Part 4 — Active Directory Configuration & Domain Join

## Objective
Install and configure Active Directory Domain Services (AD DS) on the Windows Server environment, promote the server to a Domain Controller (DC), create enterprise organizational structures, and successfully join a target Windows 10 endpoint to the newly established domain.

## Skills Learned
* Active Directory Domain Services (AD DS) Administration
* Domain Controller (DC) Deployment and Promotion
* DNS and Static IP Configuration in Enterprise Networks
* Organizational Unit (OU) and Identity Management
* Windows Domain Join and Authentication Validation

## Tools
* Windows Server
* Windows 10 VM
* Active Directory Domain Services (AD DS)
* Active Directory Users and Computers (ADUC)

## Steps

### Ref 1: Active Directory Domain Services Installation
<img src="part4-screenshots/01_ADDS.png" width="800">

Server Manager dashboard displaying Active Directory Domain Services (AD DS) and DNS roles fully installed and operational on the server, showing healthy active status indicators.

### Ref 2: Organizational Unit & Identity Architecture
<img src="part4-screenshots/02_ADUC.png" width="800">

Active Directory Users and Computers (ADUC) console management view showing the `DFIR.local` domain tree with a dedicated corporate structure, displaying the HR and IT Organizational Units (OUs) alongside the newly provisioned employee account for Dele Smith.

### Ref 3: Endpoint Domain Asset Discovery
<img src="part4-screenshots/03_ADUC_TARGET-PC.png" width="800">

Active Directory Users and Computers (ADUC) console tracking the standard Computers container, validating that the endpoint `TARGET-PC` successfully requested entry and was joined to the `DFIR.local` enterprise network.

### Ref 4: Enterprise Authentication Verification
<img src="part4-screenshots/04_Domain_Auth_CLI.png" width="800">

Command-line execution outputs checking active environment details on the workstation, confirming authenticated workspace session context as `dfir\dsmith` and system membership within the `DFIR.local` domain.

## Summary
Active Directory was successfully configured on the server, promoted to a Domain Controller for `DFIR.local`, and the Windows 10 machine successfully joined and authenticated to the domain using the Dele Smith account, confirming full domain functionality.

