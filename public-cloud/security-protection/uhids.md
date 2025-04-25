---
layout: default
title: UHIDS
parent: Security & Compliance
grand_parent: Public Cloud
permalink: /public-cloud/security-protection/uhids/
nav_order: 3
---
# UHIDS - Host Intrusion Detection
## Product overview
SCloud Host-based Intrusion Detection System, abbreviated as UHIDS (Chinese name: SCloud Host Intrusion Detection System), is an important part of SCloud. UHIDS is a host-based intrusion detection system that monitors the security of cloud hosts in real time, can timely detect hacker intrusions on cloud hosts, help users understand the security status of servers, and help customers strengthen servers.

## Explanation of terms
### Security risks
Monitor your system's server security vulnerabilities and application security vulnerabilities that can be exploited by hackers to create security risks.
### Malicious Trojans
After hacking the server is installed on the Trojan, the hacker will be able to take control of the server through the "Trojan" program. Arbitrarily destroy, steal the file, and even remotely control the seeded host.
### Offsite sign-in
If someone logs into the server from an unusual location for the server owner, consider whether the password has been cracked by a hacker.
### Brute force succeeded
The hacker matched the correct account and password by brute-forcing the account and password, and successfully logged in to the server.
### Configuration defects
Configuration defect refers to the unreasonable configuration of the server by the manager, resulting in security risks, and it is recommended that the administrator modify the server configuration to enhance the server security.
### Agent
An agent, or plug-in, is a monitoring program installed on the server

## Product advantages
### Unified security management
Supports cross-platform server management, including SCloud's virtual or physical machines, and hosts outside the platform such as Alibaba and Tencent. Regardless of the deployment environment and region, you can view and operate in the unified web console.
### Monitor risks in real time
Monitor security risks on the server in real time, such as whether the hacker is brute-forcing the login and password of the server, whether the brute force attack is successful, and whether backdoor software is installed on the server.
### Minimal resource consumption
The normal resources of the agent plugin only occupy 100Mbytes of memory<, and the CPU <5% of a single core
The agent is only responsible for information monitoring, collection, and reporting, and the analysis is carried out in the cloud protection center to minimize the occupation of system resources.
### Independent independent research and development
As a public third-party public cloud service provider, there is no operation other than detecting hacker attacks. UHIDS is an agent independently developed by SCloud, which is only used as a software program to detect hacker attacks.

## Key features
### Intrusion detection
#### SSH remote login
UHIDS collects the source address of commonly used SSH logins of users, and if SSH logins are found in the uncommon source login location, an alarm notifies the user.
#### SSH brute force
UHIDS continuously analyzes SSH logon logs, detects successful brute force attacks, and notifies users with alarms.
#### Backdoor Trojan
UHIDS detects network characteristics such as the network connection of the process, and notifies the user if a backdoor Trojan is found.
#### Abnormal process
UHIDS detects the startup directory of the process, executes the program and other processes, and alerts the user if it finds a process that is suspected to be a Trojan.
### Vulnerability detection
#### System vulnerability detection
UHIDS collects version and configuration information such as kernel version and dynamic library, compares it with historical vulnerability libraries, and notifies users with alerts if a vulnerable version is found.
#### Third-party software vulnerability detection
UHIDS collects the version information of third-party software such as Nginx, sshd, mysql, etc., compares it with the historical third-party software vulnerability library, and notifies the user with an alarm if a vulnerable version is found.
### Baseline inspection
#### Weak password verification
UHIDS periodically detects weak passwords for system accounts and mySQL accounts based on weak password dictionaries, and alerts users if weak passwords are found.
#### Application layer configuration auditing
UHIDS has a built-in security baseline library and is updated regularly, and through the reading and analysis of the configuration of application layer software (for example, `PHP\Mangodb\Redis\mysql\nginx\httpd`, etc.), determine whether the configuration items meet the configuration requirements of the security baseline, and notify users with alarms.
### Alarm management
UHIDS provides alarm management functions to facilitate users to grasp the security status of cloud hosts in the first time, and provides a whitelist mechanism for easy customization.
#### Log on to the IP address whitelist
UHIDS supports the whitelist mechanism of logging in to IP addresses.
Whitelist of login locations
UHIDS supports users to set the whitelist mechanism for logging in to cities.
#### Alarm settings
UHIDS supports alerting methods such as email and SMS, which is convenient for users to detect and deal with corresponding risks or threat events when the cloud host encounters corresponding risks or threats, reducing the security risks faced by the cloud host.
