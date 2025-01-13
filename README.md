# Active-Directory-Cyber-Red-Blue-Team

## Executive Summary
This project was designed to merge foundational knowledge of Active Directory (AD) management with practical cybersecurity applications. By setting up an AD environment and simulating real-world attack scenarios using Kali Linux, the project emphasized both defense (blue team) and offense (red team) techniques. Using Sysmon for detailed telemetry and Splunk for log analysis, the project demonstrated how to detect, analyze, and respond to security incidents in a Windows-based enterprise environment. The goal was to understand AD’s role in enterprise security while building hands-on skills in threat detection and mitigation.


## Introduction
### Background
Active Directory is central to many enterprise networks, providing authentication, authorization, and directory services. Its widespread use makes it a prime target for attackers, emphasizing the need for robust security measures. This project focuses on the dual objectives of configuring a functional AD environment and applying cybersecurity techniques to identify and respond to simulated attacks.

### Objective
The primary goals of this project were:
- To set up a secure Active Directory environment and manage its components.
- To simulate and analyze attack scenarios using tools like Kali Linux to understand potential vulnerabilities.
- To monitor, detect, and respond to threats using Sysmon and Splunk, emphasizing real-time incident detection and response.

### Scope
The project covers the setup of a Windows Server 2019-based Active Directory environment using VirtualBox, the use of Kali Linux to simulate brute force and other attack scenarios, and the integration of monitoring tools to detect and respond to these events. The scope also includes configuring group policies, managing user privileges, and analyzing telemetry data to gain insights into potential attack vectors.


## Project Overview
### Architecture Design
The virtual environment consisted of:

- Target Machine (target-pc): Connected to the emac.local domain and authenticated users via the domain controller.
- Domain Controller (ADDC01): Managed user accounts and domain policies through Active Directory.
- Splunk Server: Collected and analyzed logs for visualization and insights.
- Kali Linux Machine: Conducted simulated attacks.

### Components
- Target Machine: Windows OS integrated with the emac.local domain.
- Domain Controller: Configured with Active Directory for user and policy management.
- Splunk Enterprise: Used for log collection, analysis, and dashboard creation.
- Kali Linux: Performed penetration testing and simulated attacks.
- Sysmon: Monitored and logged system activities for analysis.

## Methodology
### 1.Active Directory Setup:

- Installed and configured Windows Server 2019 as a domain controller.
- Created organizational units (OUs), users, and groups to replicate a corporate hierarchy.
- Configured group policies for user privileges, password policies, and security settings.
- Added target machine to the domain and created domain accounts (msmith and jsmith).

### 2.Attack Simulation with Kali Linux:

- Conducted brute force attacks on AD accounts to simulate password-cracking attempts.
- Performed network scans and reconnaissance to identify potential vulnerabilities.
- Executed other red team tactics to mimic real-world adversarial behavior.

### 3.Telemetry Collection with Sysmon:

- Configured Sysmon on all endpoints to capture detailed logs, including process creation, network connections, and file modifications.
- Generated logs during attack simulations to understand the system’s responses.

### 4.Log Analysis with Splunk:

- Ingested Sysmon logs into Splunk for centralized monitoring.
- Created dashboards to visualize attack patterns and identify anomalies.
- Correlated events to reconstruct the attack timeline and assess the effectiveness of security measures.


## Testing and Validation
### Simulated Attack Scenarios
#### <ins> Brute Force Attack:</ins>

- Using the command:
#### `hydra -t 1 -w 10 -l msmith -P passwords.txt rdp://192.168.10.100`

- A brute force attack was conducted via Remote Desktop Protocol (RDP) targeting the msmith account. Upon successful login, event IDs such as 4625 (failed login), 4624 (successful login), 4634 (logoff), and 4776 (authentication attempt) were logged in Splunk. These logs provided crucial details about the attack sequence.

#### <ins> Network Scans:</ins>

- Nmap was utilized to identify open ports and services on the target machine. Sysmon captured unauthorized scanning activities, which Splunk dashboards highlighted for further analysis.


#### <ins> Installation of Atomic Red Team (ART): </ins>
To simulate MITRE ATT&CK techniques, Atomic Red Team was installed on the target Windows machine:

PowerShell Execution Policy Bypass:
The following command was executed in PowerShell (with administrator privileges) to allow scripts to run:

#### powershell:
`Set-ExecutionPolicy Bypass -Scope CurrentUser`


#### <ins> Defender Exclusions for Atomic Red Team: </ins>
- To prevent Microsoft Defender from blocking ART files, the entire C:\ drive was excluded via Windows Security:

#### Path: 
- Virus & Threat Protection > Manage Settings > Exclusions > Add Folder.
- Administrator privileges were required to confirm the exclusion.

#### <ins> Atomic Red Team Installation: </ins>
The following command downloaded and unpacked ART on the target system:

#### powershell:
`Invoke-WebRequest -Uri https://github.com/redcanaryco/atomic-red-team/archive/master.zip -OutFile atomic-red-team.zip; Expand-Archive .\atomic-red-team.zip -DestinationPath C:\AtomicRedTeam`
** Dependencies were installed by confirming prompts during setup. **

#### <ins> Testing MITRE ATT&CK Techniques: </ins>

- After installation, the C:\AtomicRedTeam\atomics directory provided access to technique IDs corresponding to MITRE ATT&CK techniques.
- For example, Technique ID T119 was explored for testing and mapped back to the MITRE ATT&CK Enterprise Matrix for further understanding.

### Log Analysis and Insights
- #### Event Detection:
  Splunk dashboards visualized real-time system activities, flagging anomalies such as repeated login attempts and unauthorized access.
- #### Correlated Events:
  Logs like Event ID 4625, 4624, 4634, and 4776 were analyzed to trace the attack path and determine its impact.
- #### Actionable Insights:
  Detailed event correlations helped identify the root cause of the attacks, enabling a better understanding of threat dynamics and response strategies.

## Conclusion
The Active Directory and Cybersecurity Project demonstrated the importance of integrating security practices with AD management. By simulating attacks and analyzing system responses, the project highlighted:

- The critical role of AD in enterprise security and its potential vulnerabilities.
- The effectiveness of telemetry tools like Sysmon in generating actionable security data.
- The value of log analysis platforms like Splunk in detecting and responding to incidents.

### Key Takeaways
- Active Directory requires continuous monitoring and robust security configurations to mitigate threats.
- Simulated attack scenarios are invaluable for understanding real-world adversarial techniques and improving defense mechanisms.
- Combining telemetry tools and SIEM platforms enhances incident detection and response capabilities.



## Figures

### Figure 1.
This diagram illustrates the logical layout of the components involved in the Active Directory and Cybersecurity project. It visualizes the flow of data and interactions between key components, including the domain controller (ADDC01), target machine, Kali Linux (used for attack simulations), and Splunk for log analysis. The diagram provides a clear overview of the project's architecture and serves as a blueprint for understanding the integration of offensive and defensive elements.


<img width="380" alt="Diagram" src="https://github.com/user-attachments/assets/e4bb13cf-cd81-40dd-af8c-a72ed9af44fc" />





### Figure 2.
A screenshot of the Active Directory interface showing the user accounts (msmith and jsmith) and organizational units.


<img width="509" alt="AD UC 3" src="https://github.com/user-attachments/assets/02a1fce2-d803-4ad3-b41c-34c98b9002f1" />

<img width="510" alt="DC Config" src="https://github.com/user-attachments/assets/7c9a40c9-c86c-42c2-a3e3-89133a5fec46" />


<img width="332" alt="Join Domain 2" src="https://github.com/user-attachments/assets/cdf0b9af-d56e-4662-90db-8b4f381932f9" />




### Figure 3.
A screenshot of the Sysmon configuration file detailing how events were logged and monitored.


<img width="369" alt="Inputs Config File" src="https://github.com/user-attachments/assets/6fbbb8d6-74ea-4497-87d5-ec217c682e7d" />


### Figure 4.
A screenshot of the Kali Linux terminal showing the Hydra brute force command and its successful execution against the RDP service.


![IMG_3944](https://github.com/user-attachments/assets/ed5cc29d-8cf3-4e7e-83c0-743d2bbce789)




### Figures 5.
A Splunk dashboard screenshot showing the detection of events.


<img width="509" alt="Splunk Event Codes" src="https://github.com/user-attachments/assets/c39fc5af-434c-40c0-955a-8d2dee4a08a0" />

<img width="509" alt="Splunk Results 3" src="https://github.com/user-attachments/assets/37423be5-822f-475f-bf7a-bba17c8434a6" />

<img width="508" alt="Splunk Results" src="https://github.com/user-attachments/assets/7b6ac826-dbd8-4470-bfe0-447ee5eafd26" />

<img width="508" alt="Splunk Results 4" src="https://github.com/user-attachments/assets/2fcddd7b-818d-4389-a3f2-81e40d1d1cce" />





### Figure 6.
This screenshot demonstrates the email notification sent to analysts as part of the Shuffle
workflow. Upon receiving a Wazuh alert, the Shuffle automation workflow not only logged the incident
in The Hive but also triggered an email notification to the designated analyst team. This step validates
the integration of Shuffle in automating response actions, ensuring that analysts are promptly notified of
security events for faster incident handling and response.


<img width="484" alt="LS3" src="https://github.com/user-attachments/assets/00f2cde8-7f75-440b-84ba-056bf3977760">



## References
SOC Automation Project. SOC Automation for Beginners [YouTube Playlist]. Retrieved from
https://www.youtube.com/playlist?list=PLG6KGSNK4PuBWmX9NykU0wnWamjxdKhDJ.

### Note: This YouTube playlist by SOC Automation for Beginners provided essential insights and practical steps for setting up and configuring a SOC environment. The videos offered a detailed, beginner-friendly approach to integrating Wazuh, TheHive, and Shuffle, making it an invaluable resource throughout the lab project.

























