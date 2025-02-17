# AD-Security-Lab-Project

## Objective
The AD Security Lab project aimed to set up an Active Directory (AD) environment, monitor security events using Splunk, and simulate real-world attacks with Atomic Red Team. This approach provided hands-on experience in domain administration, threat detection, and incident response in a controlled test environment.

### Skills Learned
- **Active Directory Administration** – Installing, configuring, and managing a Windows Server domain environment.
- **Log Management & SIEM** – Using Splunk to ingest, index, and visualize logs from domain hosts.
- **Security Hardening** – Deploying Sysmon and Windows Event Forwarding to capture valuable forensic data.
- **Threat Simulation & Analysis** – Running Atomic Red Team tests and analyzing detection capabilities.
- **Offensive Security** – Using Kali Linux (Crowbar) for brute-forcing activities in an AD environment.
- **Incident Response** – Investigating triggered alerts, refining detection rules, and documenting remediation steps.

### Tools Used
- **Windows Server (AD DS)** – For creating and managing the Active Directory domain.
- **Windows 10** – Domain-joined client for simulating user workstation activities.
- **Splunk** – SIEM solution for security event monitoring and correlation.
- **Sysmon** – Enhanced Windows logging to capture process creation, network events, and more.
- **Atomic Red Team** – Tool for simulating adversary techniques to test detection controls.
- **Kali Linux** – Attacker machine with Crowbar for brute-forcing.
- **VirtualBox/VMware** – Virtualization platform to run multiple machines on a single host.

## Steps

<img src="https://imgur.com/CVqYag9.png" alt="Imgur Image" />

*Ref 1a: Network Diagram*

## 1. Provision Virtual Machines & Network

1. **Create VMs** in VirtualBox/VMware:
   - **Windows Server 2019 or 2022** (Domain Controller)
   - **Windows 10** (Client)
   - **Splunk Server** (Linux-based)
   - **Kali Linux** (Attacker)

<img src="https://imgur.com/Izni5a2.png" alt="Imgur Image" />

*Ref 1b: Virtual Machine Setup*

2. **Assign IP Addresses** in the same subnet (e.g., 10.0.10.x) and ensure all machines can communicate.
   - Network Address Translation(NAT) was used with in Virtual box
  
<img src="https://imgur.com/zjEcrsE.png" alt="Imgur Image" />

*Ref 1c: Virtual Machine NAT Network Setup*

## 2. Configure Active Directory Domain Services

1. **Install AD DS** role in Windows Server.
   - Launch **Server Manager** → **Manage** → **Add Roles and Features** → Select **Active Directory Domain Services**.
2. **Promote Server** to Domain Controller with a domain like `drtoou.local`.

<img src="https://imgur.com/F5is10V.png" alt="Imgur Image" />

*Ref 2a: Domain Controller Setup*

3. **Configure DNS** and verify forward and reverse lookup zones.
4. **Join Windows 10** client to the `drtoou.local` domain.

<img src="https://imgur.com/2FgZwyx.png" alt="Imgur Image" />

*Ref 2b: Active Directory User Setup*


## 3. Deploy Splunk for Log Collection & Analysis

1. **Install Splunk** on a dedicated Linux VM:
   - Download the Splunk installer (deb/rpm/tar) and follow setup instructions.

<img src="https://imgur.com/3PBjjKm.png" alt="Imgur Image" />

*Ref 3a: Splunk dedicated virtual machine*
   
2. **Enable Receiving** on port 9997 in Splunk to allow Windows Forwarders to send logs.
3. **Create Indexes** (e.g., `wineventlog`, `sysmon`) for better organization.

<img src="https://imgur.com/V3oIofC.png" alt="Imgur Image" />

*Ref 3b: Splunk Web Interface*


## 4. Configure Sysmon & Universal Forwarder on Windows

1. **Install Sysmon** on both the DC and Windows 10 client:
   - `sysmon64.exe -i sysmonconfig.xml`
2. **Install Splunk Universal Forwarder**:
   - Point it to your Splunk server’s IP and port (10.0.10.10:9997).
  
<img src="https://imgur.com/AAU8HmX.png" alt="Imgur Image" />

*Ref 4a: Sysmon & Splunk Forwarder Services Running*

3. **Update Inputs** to capture:
   - Security, System, Application logs
   - Sysmon operational logs
4. **Validate** logs are arriving in Splunk.

<img src="https://imgur.com/wjQFQBg.png" alt="Imgur Image" />

*Ref 4b: Valiation that logs are coming into splunk server from **Target-PC***


## 5. Attack Simulation with Atomic Red Team

1. **Install Atomic Red Team** on the Windows 10 client.
2. **Run Attack Scenarios** such as:
   - Credential Dumping (T1003)
   - Persistence Techniques (T1050)
   - Process Injection (T1055)
   
<img src="https://imgur.com/TdXv22u.png" alt="Imgur Image" />

*Ref 4b: Invoking <a href="https://attack.mitre.org/techniques/T1003/">**T1003**</a> in the MITRE ATT&CK framework using atomic red team* 

3. **Observe Events** in Splunk:
   - Check indexes for new alerts or suspicious activity logs.
4. **Correlate Logs** with Sysmon events to confirm detection.

<img src="https://imgur.com/5MOhYG1.png" alt="Imgur Image" />

*Ref 5: Splunk logs*

## 6. Offensive Testing with Kali Linux

Using Kali Linux, I focused primarily on brute-forcing activities in the AD environment:

1. **Brute-Forcing with Crowbar**:
   - Deployed Crowbar to systematically attempt various username-password combinations.
   - Observed logs in Splunk for failed login attempts, suspicious account behaviors, and potential lockouts.

<img src="https://imgur.com/4jFfNoX.png" alt="Imgur Image" />

*Ref 6: Kali Linux Brute Force*


## 7. Alerting & Dashboards in Splunk

1. **Create Alerts** based on suspicious patterns:
   - Excessive failed logins
   - PowerShell script block logs
   - Sysmon events with known attack signatures
2. **Set Up Dashboards** to visualize top hosts, top event codes, and event timelines.
3. **Refine and Test** repeated attacks to ensure alerts trigger as intended.

<img src="https://imgur.com/Z9xFQP8.png" alt="Imgur Image" />

*Ref 7: Splunk Alert Configuration for brute force attacks*


## Conclusion

This Active Directory Security Lab project demonstrates how to configure and secure an AD environment, leverage Splunk for real-time monitoring, and test defensive measures using Atomic Red Team and **Kali Linux**. The hands-on approach with **Sysmon**, **Windows Event Forwarding**, and **Splunk** dashboards provides thorough insight into both administrative tasks and security best practices. Through repeated testing, you can continually enhance detection rules and response strategies, ensuring a robust defense posture against evolving threats.
