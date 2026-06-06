# AD Homelab Project

## Part 1 — Network Diagram

### Objective
Created a logical AD Homelab Diagram to understand network structure and telemetry flow.

### Skills Learned
* Network diagramming
* AD architecture & telemetry flow mapping

### Tools
* draw.io

### Steps

#### Ref 1: Network Diagram
<img src="diagram-part1.png" width="800">

### Lab Architecture Summary
Windows server(AD) and Windows VM (TARGET-PC) both generate telemetry via Sysmon and forward logs to Splunk using Splunk Forwarder for centralized monitoring. Kali serves as the attacker system connected to the same network for future attack simulation.

---

## Part 2 — VM Deployment

### Objective
Deployed and configured VMs required for the AD homelab environment.

### Skills Learned
* VM deployment & resource allocation
* ISO verification using SHA256 checksum
* Basic AD infrastructure understanding

### Tools
* Oracle VirtualBox

### Steps

#### Ref 1: Virtual Machine Installation
<img src="vbox-part2-overview.png" width="800">

VirtualBox manager showing 4 VMs with Ubuntu Server configuration displayed in Details tab.

### Summary
ISO files were verified using SHA256 to ensure integrity before installation. VM resources were assigned based on role, with Splunk (Ubuntu Server) allocated higher capacity for log ingestion and search performance. 
Final setup provides a stable AD lab environment for telemetry and attack simulation.

---

## Part 3 — Splunk Data Ingestion & Verification

### Objective
Installed & configured Sysmon and Splunk on both Windows VMs so they can start collecting telemetry and send logs to Splunk server for monitoring and analysis.

### Skills Learned
* Windows event logging and Sysmon telemetry generation
* Splunk Universal Forwarder deployment, inputs.conf configuration, and indexing
* NAT networking and static IP configuration in a lab environment
* Multi-host SIEM pipeline validation (Windows endpoints → Forwarder → Splunk Indexer)

### Tools
* Splunk (SIEM)
* Sysmon
* Windows VMs
* Ubuntu Server
* PowerShell

### Steps

#### Ref 1: Splunk Log Ingestion Confirmation
<img src="part3-screenshots/01_Splunk_Ingestion.png" width="800">

Splunk confirmed successful ingestion of telemetry from 2 hosts, with key log sources (Sysmon, Security, Application, System) from inputs.config. No anomalies or security incidents identified at this stage; logs represent baseline system activity.

---

## Part 4 — AD Configuration & Domain Join

### Objective
Installed and configured AD DS on Windows Server, promoted it to a DC, created organizational structures, and successfully joined a Windows 10 endpoint to the AD domain (DFIR.local).

### Skills Learned
* Active Directory Domain Services (AD DS) Administration
* Domain Controller (DC) deployment and promotion
* AD domain join and authentication validation
* DNS and Static IP configuration in enterprise networks
* Organizational Unit (OU) and identity management


### Tools
* Windows VM/Server
* AD DS
* Active Directory Users and Computers (ADUC)

### Steps

#### Ref 1: AD DS + DNS Role Installed
<img src="part4-screenshots/01_ADDS.png" width="800">

AD DS and DNS roles successfully installed and active on Windows Server.

#### Ref 2: OU Structure Created (DFIR.local)
<img src="part4-screenshots/02_ADUC.png" width="800">

ADUC shows the `DFIR.local` domain tree with a dedicated corporate structure, displaying the HR and IT OUs alongside the newly provisioned employee account for Dele Smith. User provisioning completed.

#### Ref 3: Endpoint Domain Join Verified
<img src="part4-screenshots/03_ADUC_TARGET-PC.png" width="800">

TARGET-PC successfully joined to DFIR.local domain.

#### Ref 4: Authentication Confirmed
<img src="part4-screenshots/04_Domain_Auth_CLI.png" width="800">

User session authenticated as dfir\dsmith confirming domain login.

### Summary
Active Directory was successfully configured on the server, promoted to a Domain Controller for `DFIR.local`. TARGET-PC successfully joined and authenticated to the domain using Dele Smith account, confirming full AD functionality.

---

## Part 5 — Cyber Attack Simulation & Detection

### Objective
Used Kali to perform brute-force attack against AD users and run MITRE ATT&CK techniques with Atomic Red Team, using Splunk to query the activity and identify detection gaps.

### Skills Learned
* AD attack simulation and detection analysis
* RDP brute-force attack execution (Hydra)
* Splunk log analysis and telemetry investigation
* Windows Security Event and Sysmon log monitoring
* MITRE ATT&CK technique mapping and validation
* Atomic Red Team installation and attack simulation
* Detection gap identification and visibility analysis
* PowerShell-based attack execution and monitoring

### Tools
* Kali Linux
* Hydra
* Splunk Enterprise (SIEM)
* Windows Server (Active Directory Domain Controller)
* Windows 10 Client VM
* Atomic Red Team (ART)
* Sysmon

### Steps

#### Ref 1: Hydra RDP Brute-Force Execution
<img src="part5-screenshots/01_Hydra_RDP_BF_Exec.png" width="800">

Hydra RDP brute-force attack executed against the target-pc (user: dsmith) using a reduced RockYou wordlist (first 20 entries), generating multiple login attempts over the RDP.

#### Ref 2: Splunk RDP Brute-Force Detection (EventCode 4625)
<img src="part5-screenshots/02_Splunk_RDP_BF_Detection.png" width="800">

Splunk query results showing RDP brute-force activity against WIN-NEB6ORBJ7S7, including failed logon events (EventCode 4625) and authentication telemetry correlated from Windows Security logs.

#### Ref 3: Splunk Successful Compromise Verification (EventCode 4624)
<img src="part5-screenshots/03_Splunk_4624_SuccessfulLogon_dsmith.png" width="800">

Splunk event showing successful RDP logon (EventCode 4624) for user dsmith on WIN-NEB6ORBJ7S7, including authentication and source log details confirming the brute-force succeeded.

#### Ref 4: Atomic Red Team Persistence Execution (T1136.001)
<img src="part5-screenshots/04_ART_T1136.001_NLU.png" width="800">

ART execution output for MITRE ATT&CK technique ID T1136.001, showing local user creation, privilege escalation to administrators, and cleanup on the Windows 10 VM.

#### Ref 5: Splunk Malicious Account Creation Detection
<img src="part5-screenshots/05_Splunk_T1136.001_NewLocalUser_Detection.png" width="800">

Splunk detection showing ART activity, including creation of local user account on the target machine.

### Summary

#### Detection
Splunk detected RDP brute-force activity (EventCode 4625) with 211 failed login attempts against “dsmith” account, followed by successful authentication events (4624).

#### Incident Analysis
All events occurred within the same time window, indicating clear brute-force behavior. Expanded logs showed source workstation name and IP address involved in the attempts. ART T1136.001 execution showed creation of local user “NewLocalUser” on the target system, mapped to MITRE ATT&CK persistence technique.

#### Decision Made
Both brute-force and ART activity generated true positive detections, confirming SIEM visibility across initial access and persistence stages. This enables future alert creation for similar brute-force and MITRE ATT&CK-based behaviors.
