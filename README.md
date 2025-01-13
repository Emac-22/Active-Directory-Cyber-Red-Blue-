# Active-Directory-Cyber-Red-Blue-Team

## Executive Summary
This project was designed to merge foundational knowledge of Active Directory (AD) management with practical cybersecurity applications. By setting up an AD environment and simulating real-world attack scenarios using Kali Linux, the project emphasized both defense (blue team) and offense (red team) techniques. Using Sysmon for detailed telemetry and Splunk for log analysis, the project demonstrated how to detect, analyze, and respond to security incidents in a Windows-based enterprise environment. The goal was to understand AD’s role in enterprise security while building hands-on skills in threat detection and mitigation.


## Introduction
### Background
Active Directory is central to many enterprise networks, providing authentication, authorization, and directory services. Its widespread use makes it a prime target for attackers, emphasizing the need for robust security measures. This project focuses on the dual objectives of configuring a functional AD environment and applying cybersecurity techniques to identify and respond to simulated attacks.

### Objective
The primary goals of this project were:
-To set up a secure Active Directory environment and manage its components.
-To simulate and analyze attack scenarios using tools like Kali Linux to understand potential vulnerabilities.
-To monitor, detect, and respond to threats using Sysmon and Splunk, emphasizing real-time incident detection and response.

### Scope
The project covers the setup of a Windows Server 2019-based Active Directory environment using VirtualBox, the use of Kali Linux to simulate brute force and other attack scenarios, and the integration of monitoring tools to detect and respond to these events. The scope also includes configuring group policies, managing user privileges, and analyzing telemetry data to gain insights into potential attack vectors.


## Project Overview
### Architecture Design
The virtual environment consisted of:

-Target Machine (target-pc): Connected to the emac.local domain and authenticated users via the domain controller.
-Domain Controller (ADDC01): Managed user accounts and domain policies through Active Directory.
-Splunk Server: Collected and analyzed logs for visualization and insights.
-Kali Linux Machine: Conducted simulated attacks.

### Components
-Target Machine: Windows OS integrated with the emac.local domain.
-Domain Controller: Configured with Active Directory for user and policy management.
-Splunk Enterprise: Used for log collection, analysis, and dashboard creation.
-Kali Linux: Performed penetration testing and simulated attacks.
-Sysmon: Monitored and logged system activities for analysis.

## Methodology
### 1.Active Directory Setup:

-Installed and configured Windows Server 2019 as a domain controller.
-Created organizational units (OUs), users, and groups to replicate a corporate hierarchy.
-Configured group policies for user privileges, password policies, and security settings.
-Added target machine to the domain and created domain accounts (msmith and jsmith).

### 2.Attack Simulation with Kali Linux:

-Conducted brute force attacks on AD accounts to simulate password-cracking attempts.
-Performed network scans and reconnaissance to identify potential vulnerabilities.
-Executed other red team tactics to mimic real-world adversarial behavior.

### 3.Telemetry Collection with Sysmon:

-Configured Sysmon on all endpoints to capture detailed logs, including process creation, network connections, and file modifications.
-Generated logs during attack simulations to understand the system’s responses.

### 4.Log Analysis with Splunk:

-Ingested Sysmon logs into Splunk for centralized monitoring.
-Created dashboards to visualize attack patterns and identify anomalies.
-Correlated events to reconstruct the attack timeline and assess the effectiveness of security measures.


## Testing and Validation
### Simulated Attack Scenarios
### 1.Brute Force Attack

Using the command:
hydra -t 1 -w 10 -l msmith -P passwords.txt rdp://192.168.10.100

A brute force attack was conducted via Remote Desktop Protocol (RDP) targeting the msmith account. Upon successful login, event IDs such as 4625 (failed login), 4624 (successful login), 4634 (logoff), and 4776 (authentication attempt) were logged in Splunk. These logs provided crucial details about the attack sequence.

### 2.Network Scans

Nmap was utilized to identify open ports and services on the target machine. Sysmon captured unauthorized scanning activities, which Splunk dashboards highlighted for further analysis.

### 3.Privilege Escalation Attempts

Simulated scenarios of attackers attempting to elevate privileges. Logs from these activities were processed and correlated in Splunk, providing insights into potential vulnerabilities.

### Log Analysis and Insights
-Event Detection: Splunk dashboards visualized real-time system activities, flagging anomalies such as repeated login attempts and unauthorized access.
-Correlated Events: Logs like Event ID 4625, 4624, 4634, and 4776 were analyzed to trace the attack path and determine its impact.
-Actionable Insights: Detailed event correlations helped identify the root cause of the attacks, enabling a better understanding of threat dynamics and response strategies.

## Conclusion
The Active Directory and Cybersecurity Project demonstrated the importance of integrating security practices with AD management. By simulating attacks and analyzing system responses, the project highlighted:

-The critical role of AD in enterprise security and its potential vulnerabilities.
-The effectiveness of telemetry tools like Sysmon in generating actionable security data.
-The value of log analysis platforms like Splunk in detecting and responding to incidents.

### Key Takeaways
-Active Directory requires continuous monitoring and robust security configurations to mitigate threats.
-Simulated attack scenarios are invaluable for understanding real-world adversarial techniques and improving defense mechanisms.
-Combining telemetry tools and SIEM platforms enhances incident detection and response capabilities.



## Figures

### Figure 1.
This diagram illustrates the logical layout of the components involved in the SOC Automation project. It
visualizes the flow of data between key components such as the Windows client, Wazuh manager, The
Hive, Shuffle, and the SOC analyst. The diagram is essential for understanding the project’s architecture
and serves as a blueprint for the subsequent setup and integration steps.


<img width="330" alt="LS9" src="https://github.com/user-attachments/assets/610909e9-ccdf-4b49-b1e8-296e5aba1eaf">



### Figure 2.
This screenshot displays the Wazuh dashboard showing the droplets created for the Wazuh
server and TheHive server. The droplets are virtual machines (VMs) that have been set up to host the
Wazuh and TheHive platforms, which are integral to the SOC Automation project. The configuration of
these droplets allows for the management and monitoring of security events, enabling effective incident
response and automation workflows.


<img width="929" alt="LS8" src="https://github.com/user-attachments/assets/7508e216-b537-4d1d-9dbb-d2c5b5e8a45a">


### Figure 3.
This screenshot shows the execution of the Mimikatz command in PowerShell with
administrative privileges. Mimikatz is used here to simulate a security event, which is then captured by
Wazuh for telemetry configuration. The command demonstrates the process of generating test events to
help validate the functionality of the Wazuh platform’s detection capabilities for specific threats, such as
credential dumping. This step is crucial in configuring and fine-tuning the telemetry within the SOC
automation workflow.


<img width="450" alt="LS6" src="https://github.com/user-attachments/assets/7db1b387-6294-4c61-82df-0b63d513cdbe">

### Figure 4.
This screenshot showcases the workflow built in Shuffle.io for automating the detection and
response processes. The workflow integrates Wazuh alerts with The Hive, enabling automated responses
when a threat is detected. In this case, the workflow specifically handles Mimikatz detection, extracting
file hashes and checking their reputation on VirusTotal, before sending the data to The Hive for further
case management. This step demonstrates how Shuffle.io is utilized to streamline incident response and
automate security processes within the SOC environment.


<img width="895" alt="LS2" src="https://github.com/user-attachments/assets/45218820-8f00-4c80-995a-913fa6d23777">


### Figures 5.
This screenshot illustrates the alert that was automatically created in The Hive dashboard as
part of the Shuffle workflow integration. Upon detecting Mimikatz activity on the endpoint—simulated
by running Mimikatz in PowerShell as part of the test—Wazuh triggered an alert. This alert was then
routed to The Hive, where a case was automatically generated for further investigation and response.
This process demonstrates the seamless integration of Wazuh, Shuffle, and The Hive for automated
incident management in the SOC environment.


<img width="954" alt="LS4" src="https://github.com/user-attachments/assets/64624018-31ee-4c9f-b2b7-755a147095db">



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

























