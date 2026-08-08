# Slow Computer Troubleshooting Runbook

## Overview

This runbook provides a standardized procedure for troubleshooting **slow computer performance** in an IT support environment.

A slow computer can result from high CPU or memory usage, insufficient disk space, storage performance problems, excessive startup applications, background processes, Windows system issues, malware, network delays, hardware limitations, or application-specific problems.

The objective of this runbook is to identify the underlying cause of poor computer performance, apply an appropriate resolution, verify system performance, and determine when escalation is required.

---

## Purpose

This procedure is intended to help IT Support and Service Desk personnel:

* Diagnose slow Windows computer performance
* Identify CPU, memory, disk, and network bottlenecks
* Troubleshoot slow application performance
* Identify excessive background processes
* Investigate startup and login delays
* Check available storage
* Troubleshoot Windows system performance issues
* Identify potential hardware problems
* Identify potential security-related performance issues
* Determine when performance issues require escalation

---

## Scope

This runbook applies to common performance problems involving:

* Windows desktop computers
* Windows laptops
* Business applications
* Startup and login performance
* CPU utilization
* Memory utilization
* Disk utilization
* Storage capacity
* Network-dependent applications
* Background processes
* Windows services
* Hardware performance

The exact troubleshooting process may vary depending on the organization's hardware configuration, endpoint management solution, security software, and business applications.

---

# Common Slow Computer Symptoms

Users may report:

* Computer takes a long time to start
* Windows login is slow
* Applications take a long time to open
* Applications freeze or become unresponsive
* Computer frequently displays "Not Responding"
* Typing or mouse movement is delayed
* Files take a long time to open
* Browser performance is poor
* Computer becomes slow after running for some time
* Fan runs continuously
* Computer becomes unusually hot
* Disk usage remains high
* Computer becomes slow when connected to a network
* Computer performance has gradually declined over time

---

# Troubleshooting Procedure

## 1. Identify the Problem

Determine exactly what the user means by "slow."

Ask and document:

* What is slow?
* When did the problem begin?
* Is the problem constant or intermittent?
* Does the problem occur after startup or after extended use?
* Which applications are affected?
* Does the entire computer become slow?
* Does the problem occur only when connected to the network?
* Has new software or hardware recently been installed?
* Has Windows recently been updated?
* Has the user recently changed their workflow?

Determine whether the problem affects:

* Windows itself
* A specific application
* File access
* Network resources
* Internet access
* Startup
* Login
* Shutdown

---

## 2. Determine the Scope

Establish whether the issue is limited to one computer or affects multiple users.

### One computer affected

Possible causes include:

* Hardware limitations
* Local software
* Background processes
* Storage problems
* Windows configuration
* Malware
* Corrupted system files
* Insufficient resources

### Multiple computers affected

Possible causes include:

* Network performance
* Shared application infrastructure
* Server performance
* Cloud service issues
* Software deployment
* Security policy changes
* Organization-wide system changes

If multiple users report the same performance problem, investigate the possibility of a wider incident rather than treating each workstation independently.

---

# Basic Performance Checks

## 3. Restart the Computer

If the computer has been running for an extended period, perform a controlled restart where appropriate.

A restart can clear:

* Temporary processes
* Memory utilization
* Hung applications
* Pending updates
* Temporary system conditions

Do not assume that restarting is the complete solution. If the issue returns, continue troubleshooting to identify the underlying cause.

---

## 4. Check Task Manager

Open **Task Manager** and review system resource utilization.

Check:

* CPU
* Memory
* Disk
* Network
* Processes
* Startup applications

Look for processes consistently consuming excessive resources.

High resource utilization should be correlated with the user's symptoms before terminating processes.

Do not terminate unknown system or security processes without understanding their purpose and authorization.

---

# CPU Troubleshooting

## 5. Check CPU Utilization

Review CPU usage in Task Manager.

High CPU utilization may be caused by:

* Resource-intensive applications
* Background processes
* Windows updates
* Antivirus scans
* Browser processes
* Software installation
* Application errors
* Malware
* Hardware limitations

Identify which process is consuming CPU resources.

Determine whether high CPU usage is:

* Temporary
* Persistent
* Associated with a specific application
* Associated with system activity

---

## 6. Investigate High CPU Usage

If a specific application is consuming excessive CPU:

1. Confirm whether the application is required.
2. Check whether the application is performing a known resource-intensive task.
3. Check for application updates.
4. Restart the application where appropriate.
5. Check whether the issue returns.
6. Review application logs where available.

If a Windows process is consuming excessive CPU, investigate the process before terminating it.

---

# Memory Troubleshooting

## 7. Check Memory Utilization

Review memory usage in Task Manager.

High memory usage can cause:

* Application slowdowns
* Excessive paging
* Application crashes
* System freezes
* Delayed response times

Identify applications consuming significant amounts of memory.

Common contributors include:

* Web browsers
* Large spreadsheets
* Virtual machines
* Development tools
* Collaboration applications
* Background services

---

## 8. Check for Memory Pressure

If memory usage remains consistently high:

1. Identify the applications consuming memory.
2. Close unnecessary applications where appropriate.
3. Check whether the applications have known memory issues.
4. Restart the affected application.
5. Restart the computer if required.
6. Determine whether the device has sufficient physical memory for the user's workload.

Repeated high memory usage may indicate that the device requires hardware evaluation or workload optimization.

---

# Disk Troubleshooting

## 9. Check Disk Utilization

Review disk utilization in Task Manager.

Consistently high disk activity may be caused by:

* Windows Update
* Antivirus scans
* Application activity
* File synchronization
* Search indexing
* Insufficient memory
* Storage problems
* Background processes

Identify the process generating the disk activity.

---

## 10. Check Available Disk Space

Low storage capacity can affect system performance and prevent applications or Windows from operating correctly.

Check available space on the system drive.

Investigate:

* Temporary files
* Recycle Bin
* Downloads
* Large user files
* Application caches
* Windows update files
* Local synchronization folders

Do not delete business data or system files without confirming their purpose.

---

## 11. Check Storage Health

If performance problems persist and disk activity appears abnormal, investigate storage health.

Potential indicators of a storage problem include:

* Extremely slow file operations
* Frequent application freezes
* Unexpected system errors
* Disk-related warnings
* Files taking unusually long to open
* Repeated Windows crashes

Hardware diagnostics should follow the organization's approved procedures.

If storage failure is suspected, prioritize data protection and escalation.

---

# Startup and Login Performance

## 12. Review Startup Applications

Excessive startup applications can significantly increase boot and login times.

Review startup applications in Task Manager.

Identify applications that:

* Are unnecessary
* Start automatically
* Consume significant resources
* Are no longer required

Disable startup applications only when appropriate and permitted by organizational policy.

Do not disable security, management, or business-critical applications without authorization.

---

## 13. Investigate Slow Windows Login

If the computer becomes slow specifically during login, investigate:

* Group Policy processing
* Login scripts
* Network connectivity
* Mapped drives
* Roaming or redirected profiles where applicable
* Authentication services
* Startup applications
* Endpoint management agents
* Security software
* Network resource availability

If multiple users experience slow login times, investigate domain or infrastructure-level causes.

---

# Application-Specific Performance

## 14. Determine Whether One Application Is Affected

If only one application is slow:

1. Test another application.
2. Determine whether Windows itself is responsive.
3. Restart the affected application.
4. Check application updates.
5. Check application resource usage.
6. Review application configuration.
7. Check network connectivity if the application is network-dependent.
8. Test with an appropriate alternative user profile where authorized.

If other applications perform normally, focus troubleshooting on the affected application rather than the entire operating system.

---

# Network-Related Performance

## 15. Determine Whether the Network Is the Cause

Some applications appear slow because they depend on network resources.

Examples include:

* Cloud applications
* File shares
* Remote desktops
* VPN applications
* Network databases
* Web-based business applications

Test general connectivity:

```text id="1f2y8h"
ping <default-gateway>
```

Test DNS:

```text id="x1i8h4"
nslookup example.com
```

If the application is only slow when accessing a network resource, investigate the network path rather than assuming that the computer itself is slow.

For wireless-related problems, follow the [Wi-Fi Connectivity](wi-fi-connectivity.md) runbook.

For VPN-related performance problems, follow the [VPN Troubleshooting](vpn-troubleshooting.md) runbook.

---

# Windows System Troubleshooting

## 16. Check Windows Updates

Pending or recently installed Windows updates can affect system performance.

Check:

* Pending updates
* Recent updates
* Update installation status
* Restart requirements
* Failed updates

If performance changed immediately after an update, document the timing and investigate whether the update is related.

Do not remove security updates without authorization.

---

## 17. Check Windows System Files

If Windows appears unstable or corrupted, use approved system integrity tools.

Run an elevated Command Prompt or PowerShell session.

Check system file integrity:

```text id="7l8k2d"
sfc /scannow
```

If required, use the Deployment Image Servicing and Management tool:

```text id="d3b5n0"
DISM /Online /Cleanup-Image /RestoreHealth
```

Allow each command to complete before evaluating the results.

Record relevant output in the support ticket without including sensitive information.

---

# Malware and Security Considerations

## 18. Check for Security-Related Performance Issues

Unexpected performance degradation may be associated with:

* Malware
* Suspicious processes
* Unauthorized software
* Cryptomining
* Security software activity
* Unusual network activity

Follow organizational endpoint security procedures.

Do not disable antivirus or endpoint protection merely to determine whether it is causing performance problems.

If malicious activity is suspected, stop routine troubleshooting and follow the organization's security incident response process.

---

# Hardware Considerations

## 19. Check Hardware Limitations

Consider the computer's hardware specifications relative to the user's workload.

Check:

* RAM
* CPU
* Storage type
* Storage capacity
* Device age
* Thermal conditions
* Hardware health

A computer may function normally but still be unsuitable for a user's workload because of insufficient hardware resources.

Examples include:

* Insufficient RAM for large applications
* Older mechanical hard drive
* Limited CPU performance
* Insufficient storage capacity
* Thermal throttling

Hardware upgrades or replacement should follow organizational asset management procedures.

---

# Thermal and Power Issues

## 20. Check for Overheating

Possible symptoms of thermal problems include:

* Loud or continuously running fans
* System slowdown after extended use
* Unexpected shutdowns
* High temperatures
* Performance degradation under load

Check for:

* Blocked ventilation
* Dust buildup
* Docking station or power issues
* Environmental conditions
* Hardware diagnostic warnings

Hardware cleaning or internal repairs should follow approved procedures and manufacturer guidance.

---

# Advanced Troubleshooting

## 21. Check Event Viewer

Windows Event Viewer can provide useful information when performance problems are associated with system errors.

Review relevant logs for:

* Disk errors
* Application errors
* Service failures
* Driver problems
* Unexpected shutdowns
* Hardware-related warnings

Pay particular attention to events that occur at the same time as the reported performance issue.

Avoid treating every warning as a root cause. Correlate event timestamps with the user's symptoms.

---

## 22. Check Reliability Monitor

Windows Reliability Monitor can help identify patterns of:

* Application crashes
* Windows failures
* Driver failures
* Hardware problems
* Update failures

Look for failures that correlate with when the performance issue began.

This can be particularly useful when the problem is intermittent.

---

# Common Slow Computer Scenarios

| Symptom                                    | Possible Cause                                    | Recommended Investigation                                        |
| ------------------------------------------ | ------------------------------------------------- | ---------------------------------------------------------------- |
| Computer is slow immediately after startup | Startup applications or updates                   | Check startup programs and Windows Update                        |
| Applications are slow                      | CPU, memory, disk, or application issue           | Review Task Manager and application behavior                     |
| Disk usage remains at 100%                 | Background process, storage, or memory pressure   | Identify disk-consuming processes and check storage health       |
| Memory usage remains high                  | Too many applications or insufficient RAM         | Identify memory-heavy processes and evaluate workload            |
| Computer becomes slow over time            | Memory leak, background process, or thermal issue | Restart, monitor resources, and investigate recurring processes  |
| Only one application is slow               | Application-specific problem                      | Test other applications and investigate the affected application |
| Files open slowly                          | Storage or network issue                          | Determine whether files are local or network-based               |
| Login is slow                              | Group Policy, scripts, network, or profile issue  | Investigate login dependencies                                   |
| Computer is slow only on Wi-Fi             | Network issue                                     | Follow Wi-Fi troubleshooting procedures                          |
| Computer is slow only over VPN             | VPN or network performance                        | Follow VPN troubleshooting procedures                            |
| Fan runs continuously                      | High CPU, thermal issue, or background activity   | Check processes and hardware conditions                          |
| Performance degraded after an update       | Update or driver issue                            | Review recent updates and document timing                        |

---

# Verification Checklist

* [ ] User symptoms documented
* [ ] Scope of issue determined
* [ ] Computer restarted where appropriate
* [ ] CPU utilization checked
* [ ] Memory utilization checked
* [ ] Disk utilization checked
* [ ] Available storage checked
* [ ] Startup applications reviewed
* [ ] Application-specific performance checked
* [ ] Network dependency evaluated
* [ ] Windows Update status checked
* [ ] System integrity checked where appropriate
* [ ] Event Viewer reviewed where appropriate
* [ ] Reliability Monitor reviewed where appropriate
* [ ] Security considerations evaluated
* [ ] Hardware limitations considered
* [ ] Resolution applied
* [ ] Performance verified
* [ ] User confirms improvement
* [ ] Ticket updated
* [ ] Escalation completed if required

---

# Escalation Criteria

Escalate the issue to the appropriate desktop support, systems, infrastructure, security, or hardware team when:

* Hardware failure is suspected
* Storage failure is suspected
* Multiple users experience the same performance problem
* A shared application is affected
* Network infrastructure appears to be responsible
* The computer repeatedly crashes
* System files cannot be repaired
* Malware or unauthorized activity is suspected
* Performance remains poor after approved troubleshooting
* Hardware replacement or upgrade is required
* Administrative changes outside the support team's authorization are required

Provide the escalation team with:

* Device information
* Operating system
* User symptoms
* When the problem began
* Resource utilization findings
* Application information
* Error messages
* Event or Reliability Monitor findings
* Troubleshooting performed
* Actions already taken
* Test results
* Number of affected users where applicable

---

# Documentation Requirements

Record the following information in the support ticket:

* Date and time reported
* Affected user
* Device name or asset identifier according to organizational policy
* Operating system
* Symptoms
* Applications affected
* Resource utilization findings
* Storage findings
* Network-related findings
* Error messages
* Troubleshooting performed
* Changes made
* Resolution
* Verification results
* Escalation details where applicable

Do not record passwords, authentication tokens, recovery keys, or other sensitive information.

---

# Prevention

Organizations can reduce recurring computer performance problems by:

* Maintaining supported hardware standards
* Monitoring endpoint health
* Keeping Windows and approved applications updated
* Maintaining sufficient disk space
* Reviewing unnecessary startup applications
* Monitoring endpoint security
* Replacing aging hardware when appropriate
* Maintaining adequate RAM and storage capacity
* Monitoring recurring application performance issues
* Maintaining appropriate endpoint management policies
* Educating users about safe software installation and device maintenance
* Documenting recurring performance problems and their root causes

---

# Related Runbooks

* [Wi-Fi Connectivity](wi-fi-connectivity.md)
* [VPN Troubleshooting](vpn-troubleshooting.md)
* [Blue Screen of Death (BSOD)](blue-screen-of-death-bsod.md)
* [Laptop Setup Checklist](laptop-setup-checklist.md)
* [Incident Escalation](incident-escalation.md)
