# Windows Internals Security Analysis (Blue Team Project)

## 📌 Project Overview
This project focuses on understanding critical Windows system processes
from a defensive cybersecurity (Blue Team) perspective.

The goal is to establish a baseline of normal behavior and identify
suspicious activity that could indicate attacks such as credential
dumping or process masquerading.

## 🎯 Objectives
- Understand core Windows processes and their security relevance
- Analyze parent–child process relationships
- Differentiate normal vs suspicious process behavior
- Build a Blue Team / SOC analyst mindset

## Tools & Environment

- Operating System: Windows 11
- Analysis Tool: Sysinternals Process Explorer
- Purpose: To analyze Windows internal processes from a security and defensive perspective
- Windows Event Viewer
- PowerShell (basic commands)


## 📂 Project Structure
This repository will include:
- Normal behavior analysis of Windows processes
- Suspicious behavior simulations (safe & legal)
- Event log observations
- Detection-thinking notes
- Lessons learned

## 🚀 Why This Project Matters
Understanding normal Windows process behavior is crucial for defensive
security analysts to differentiate between legitimate and malicious activity.
This baseline knowledge helps detect attacks like credential theft, privilege
escalation, and process injection.

## Windows Internals Analysis
### Day 1 – Project Setup & Basics

- Created GitHub repository for documenting learning
- Set up README structure
- Initial exploration of Windows processes
- Understood the concept of parent-child process relationships

### Day 2 – Core Windows Processes

- Studied core Windows processes such as:
  - System
  - smss.exe
  - csrss.exe
  - wininit.exe
- Observed parent-child relationships using Process Explorer
- Learned why some system processes show "Access Denied" paths
- Understood why certain processes do not have verified signatures
- Focused on identifying normal behavior vs suspicious patterns

### Day 3 – Security-Critical Windows Processes

- Focus on security-sensitive processes such as:
  - lsass.exe
  - winlogon.exe
  - services.exe
- Analyze why these processes are high-value targets for attackers
- Perform defensive observations using Process Explorer

#### lsass.exe – Local Security Authority Subsystem Service

- Image Path: Access Denied (protected system process)
- Verified Signer: Not shown (expected for protected system process)
- Parent Process: wininit.exe
- User Account: NT AUTHORITY\SYSTEM
- Instances: 1

**Security Relevance:**
lsass.exe is responsible for authentication and credential handling in Windows. 
It is a high-value target for attackers, which is why Windows restricts access 
to this process and hides certain details. LSASS is often targeted for credential dumping attacks because attackers try
to read credentials directly from its memory.

**Observations:**
The process behavior observed matches expected normal behavior. No suspicious 
patterns such as multiple instances, unusual parent processes, or user-level 
execution were detected.

**Process Explorer Observation:**

![lsass.exe Process Explorer](screenshots/day3-lsass.png)

#### winlogon.exe – Windows Logon Process

- Image Path: C:\Windows\System32\winlogon.exe
- Verified Signer: Microsoft Windows
- Parent Process: Non-existent Process
- User Account: NT AUTHORITY\SYSTEM
- Instances: 1

**Security Relevance:**
winlogon.exe manages user logon and authentication in Windows. It requires 
highest privileges (SYSTEM) and is a key target for attackers attempting 
to manipulate logon sessions or bypass authentication.

**Observations:**
The process behavior observed matches expected normal behavior. No suspicious 
patterns such as incorrect path, multiple instances, or abnormal user execution were detected.

![winlogon.exe Process Explorer](screenshots/day3-winlogon.png)

#### services.exe – Service Control Manager

- Image Path: Access Denied (protected system process)
- Verified Signer: Not shown (expected for protected system process)
- Parent Process: wininit.exe
- User Account: NT AUTHORITY\SYSTEM
- Instances: 1

**Security Relevance:**
services.exe is responsible for managing Windows services. It is a critical 
system process that runs with SYSTEM privileges. Attackers may target it 
for service manipulation or persistence techniques.

**Observations:**
The process behavior observed matches expected normal behavior. No suspicious 
patterns such as incorrect path, multiple instances, or abnormal user execution were detected.

![services.exe Process Explorer](screenshots/day3-services.png)

### Day 4 – Process Monitoring & Baseline Analysis

#### Step 1: Observing Normal Process Behavior

The goal of this step was to establish a baseline for CPU and memory usage of key Windows processes.

**Observations:**

- **lsass.exe**
  - CPU: Mostly idle, occasional small spikes
  - Memory: Stable, no rapid increase
- **winlogon.exe**
  - CPU: Idle
  - Memory: Stable
- **services.exe**
  - CPU: Mostly idle
  - Memory: Stable
- **svchost.exe** (example instance)
  - CPU: Low usage
  - Memory: Stable

**Summary:**  
All critical processes were operating within expected ranges. No abnormal spikes or unusual memory behavior were observed.

![Process Explorer showing baseline CPU and memory usage](screenshots/day4-process-baseline.png)


