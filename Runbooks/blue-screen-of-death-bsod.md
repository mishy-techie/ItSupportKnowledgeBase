# Blue Screen of Death (BSOD) Troubleshooting Runbook

## Overview

This runbook provides a standardized procedure for troubleshooting the **Windows Blue Screen of Death (BSOD)**, also known as a **stop error** or **Windows stop code error**.

A BSOD occurs when Windows encounters a critical system condition that prevents it from continuing safely. The underlying cause may involve faulty hardware, device drivers, Windows system files, software conflicts, storage problems, memory issues, firmware, or other system-level conditions.

The objective of this runbook is to identify the stop error, collect useful diagnostic information, determine the likely root cause, restore system stability, and escalate the issue when required.

---

## Purpose

This procedure is intended to help IT Support and Service Desk personnel:

* Diagnose Windows BSOD errors
* Identify Windows stop codes
* Collect information before restarting or modifying the system
* Investigate driver-related crashes
* Check hardware and system integrity
* Troubleshoot recurring blue-screen errors
* Determine whether the issue is software or hardware related
* Verify system stability after remediation
* Identify when escalation is required

---

## Scope

This runbook applies to Windows computers experiencing:

* Blue Screen of Death errors
* Windows stop errors
* Unexpected system crashes
* Automatic restarts following critical errors
* Repeated Windows crashes
* Driver-related crashes
* Hardware-related system failures

The exact troubleshooting process may vary depending on the Windows version, device manufacturer, hardware configuration, endpoint management platform, and available diagnostic information.

---

# Common BSOD Symptoms

Users may report:

* Blue screen with a stop code
* Computer unexpectedly restarts
* Computer crashes during startup
* Computer crashes while using a specific application
* Computer crashes after installing a driver
* Computer crashes after a Windows update
* Repeated crashes
* System becomes unstable after waking from sleep
* Computer displays a stop error and cannot boot normally

Common stop codes may include:

* `CRITICAL_PROCESS_DIED`
* `SYSTEM_SERVICE_EXCEPTION`
* `IRQL_NOT_LESS_OR_EQUAL`
* `PAGE_FAULT_IN_NONPAGED_AREA`
* `MEMORY_MANAGEMENT`
* `DRIVER_IRQL_NOT_LESS_OR_EQUAL`
* `KMODE_EXCEPTION_NOT_HANDLED`
* `INACCESSIBLE_BOOT_DEVICE`

A stop code alone does not always identify the root cause. It should be considered together with recent system changes, drivers, hardware symptoms, logs, and dump files.

---

# Troubleshooting Procedure

## 1. Identify the Incident

Determine:

* When the BSOD occurred
* Whether it has happened previously
* What the user was doing when it occurred
* Whether the computer restarted automatically
* Whether a stop code was displayed
* Whether a driver or file name was displayed
* Whether the issue occurs during startup or normal operation
* Whether new hardware or software was recently installed
* Whether Windows or driver updates were recently installed

Record the exact stop code whenever possible.

---

## 2. Determine Whether the Computer Can Start Normally

After the crash, determine whether Windows can boot normally.

### If Windows starts normally

Continue with diagnostic and system integrity checks.

### If Windows repeatedly crashes during startup

Use Windows recovery and startup troubleshooting procedures.

Potential options include:

* Safe Mode
* Startup Repair
* System Restore where available and appropriate
* Windows Recovery Environment
* Advanced startup options

Avoid performing destructive recovery procedures before confirming that required data has been protected.

---

## 3. Capture the Stop Code

If the blue screen appears again, record:

* Stop code
* Error message
* Referenced driver or file
* Timestamp
* Actions being performed before the crash

If available, capture a screenshot or photograph of the BSOD for diagnostic purposes while ensuring that no sensitive information is exposed.

---

# Recent Changes

## 4. Check for Recent System Changes

Determine whether the issue began after:

* Windows Update
* Driver installation
* BIOS or firmware update
* Application installation
* Security software update
* Hardware installation
* RAM replacement
* Storage replacement
* Peripheral installation
* Configuration change

A strong correlation between a recent change and the first BSOD can help identify the likely cause.

---

# Windows Event Logs

## 5. Check Event Viewer

Open **Event Viewer** and review relevant system logs.

Look for events around the time of the crash.

Useful areas include:

```text
Windows Logs
└── System
```

Pay attention to:

* Critical events
* Error events
* Driver-related errors
* Hardware-related warnings
* Unexpected shutdowns
* Service failures

The event log may contain useful information about what occurred immediately before or after the crash.

Do not assume that every critical or error event is the root cause. Correlate timestamps and symptoms.

---

# Reliability Monitor

## 6. Review Reliability Monitor

Windows Reliability Monitor can provide a timeline of:

* Application crashes
* Windows failures
* Hardware failures
* Driver failures
* Updates
* Installation events

Look for failures occurring around the time of the BSOD.

Repeated failures involving the same application, driver, or component may provide useful evidence.

---

# Memory Dump Analysis

## 7. Check for Crash Dump Files

Windows may create memory dump files following a BSOD.

Common locations include:

```text
C:\Windows\Minidump
```

and:

```text
C:\Windows\MEMORY.DMP
```

The exact dump configuration depends on the Windows system settings.

Dump files can contain information useful for identifying:

* Faulting drivers
* Kernel components
* Memory-related failures
* System processes
* Stop error information

Do not upload crash dumps containing sensitive information to unauthorized services.

---

# Driver Troubleshooting

## 8. Identify Recently Changed Drivers

Drivers are a common source of Windows system crashes.

Investigate recently installed or updated drivers for:

* Graphics adapters
* Network adapters
* Storage controllers
* Chipsets
* Audio devices
* Docking stations
* USB devices
* Security software
* Virtualization software

Check Device Manager for:

* Warning symbols
* Missing devices
* Driver errors
* Recently changed devices

---

## 9. Update or Roll Back Drivers

If a driver is strongly suspected:

1. Identify the affected hardware.
2. Determine the current driver version.
3. Check whether the issue began after a driver update.
4. Obtain the approved driver from the appropriate source.
5. Update the driver where appropriate.
6. If the problem began immediately after an update, evaluate whether an approved rollback is appropriate.
7. Restart the computer.
8. Monitor system stability.

Do not install unapproved drivers or third-party driver-management software.

---

# Windows System File Troubleshooting

## 10. Run System File Checker

Corrupted Windows system files can contribute to system instability.

Open an elevated Command Prompt or PowerShell session and run:

```text id="4y4b0r"
sfc /scannow
```

Allow the scan to complete.

Review the result and document whether corrupted files were found and repaired.

---

## 11. Run DISM

If required, use Deployment Image Servicing and Management:

```text id="y1f1qq"
DISM /Online /Cleanup-Image /RestoreHealth
```

Allow the operation to complete before restarting the computer.

After DISM completes, run:

```text id="1h8tqs"
sfc /scannow
```

again where appropriate.

---

# Hardware Troubleshooting

## 12. Check Memory

Faulty RAM can cause:

* `MEMORY_MANAGEMENT`
* `PAGE_FAULT_IN_NONPAGED_AREA`
* Random crashes
* Application failures
* System instability

Run the organization's approved memory diagnostic procedure.

Windows Memory Diagnostic may be used where appropriate.

If memory errors are detected, escalate for hardware replacement or repair.

---

## 13. Check Storage

Storage problems can cause:

* System crashes
* Corrupted files
* Slow performance
* Boot failures
* `INACCESSIBLE_BOOT_DEVICE`
* Unexpected application errors

Check for:

* Disk errors
* Storage health warnings
* SMART alerts where available
* File system errors
* Unusual disk behavior

If storage failure is suspected, prioritize data protection and escalate appropriately.

---

## 14. Check Hardware Changes

Ask whether the device recently received:

* Additional RAM
* New SSD or HDD
* Docking station
* External device
* Graphics hardware
* Network adapter
* USB device

Temporarily removing newly installed hardware may help determine whether the change is related, provided this is permitted by organizational procedures.

---

# Safe Mode Troubleshooting

## 15. Test Safe Mode

If Windows crashes during normal operation, Safe Mode can help determine whether a third-party driver or startup application is involved.

If the computer operates normally in Safe Mode but crashes during normal Windows operation, investigate:

* Third-party drivers
* Startup applications
* Security software
* Recently installed applications
* Device services

Do not assume that Safe Mode automatically identifies the cause. Use the result as diagnostic evidence.

---

# Application-Specific BSODs

## 16. Determine Whether a Specific Application Triggers the Crash

If the BSOD consistently occurs while using a particular application:

1. Determine whether the application is fully updated.
2. Check application compatibility.
3. Review application logs where available.
4. Check related drivers.
5. Check system resource usage.
6. Test whether the issue occurs outside the application.
7. Review recent application changes.

If only one application consistently triggers the crash, escalate to the appropriate application or systems team when required.

---

# Windows Update and BSOD Troubleshooting

## 17. Check Recent Windows Updates

Determine whether the BSOD began after:

* Windows quality update
* Feature update
* Security update
* Driver update

Review Windows Update history.

If there is strong evidence that a recent update is related, follow the organization's approved update rollback or remediation procedure.

Do not remove security updates without authorization.

---

# Security Considerations

## 18. Investigate Suspicious System Behavior

Unexpected crashes can occasionally be associated with:

* Malware
* Rootkits
* Unauthorized drivers
* Endpoint security conflicts
* Suspicious software
* System compromise

If suspicious activity is identified:

1. Stop routine troubleshooting.
2. Preserve relevant diagnostic information.
3. Follow the organization's security incident response procedure.
4. Escalate to the security team.

Do not disable endpoint protection simply to test whether it is related to the crash.

---

# Recurring BSOD Troubleshooting

## 19. Identify Patterns

For recurring crashes, document:

* Stop code
* Frequency
* Date and time
* User activity
* Application being used
* Recent changes
* Driver versions
* Hardware configuration
* Dump file information
* Event Viewer findings

Look for patterns between multiple incidents.

If multiple computers experience the same BSOD after a common software, driver, or policy change, investigate the possibility of a wider incident.

---

# Recovery Considerations

## 20. Protect User Data

Before performing major recovery actions:

* Confirm important data is backed up
* Verify cloud synchronization where applicable
* Protect business-critical files
* Follow organizational data recovery procedures

Do not immediately reimage or reset a computer when diagnostic evidence may still be required to determine the root cause.

---

## 21. System Restore or Recovery

Where appropriate and authorized, consider:

* System Restore
* Startup Repair
* Windows Recovery Environment
* Driver rollback
* Approved system recovery procedures

Recovery actions should be selected based on the identified problem and organizational procedures.

---

# Common BSOD Scenarios

| Symptom / Stop Code              | Possible Cause                         | Recommended Investigation                                 |
| -------------------------------- | -------------------------------------- | --------------------------------------------------------- |
| `MEMORY_MANAGEMENT`              | RAM or memory-related problem          | Run memory diagnostics and review dump information        |
| `PAGE_FAULT_IN_NONPAGED_AREA`    | Driver, RAM, or system file issue      | Check recent drivers, memory, and system integrity        |
| `IRQL_NOT_LESS_OR_EQUAL`         | Commonly driver or memory-related      | Review recent drivers and dump information                |
| `SYSTEM_SERVICE_EXCEPTION`       | Driver, system file, or software issue | Check recent changes, drivers, and system integrity       |
| `CRITICAL_PROCESS_DIED`          | Critical Windows process failure       | Review system files, drivers, and crash information       |
| `DRIVER_IRQL_NOT_LESS_OR_EQUAL`  | Driver problem                         | Identify the referenced driver and investigate            |
| `KMODE_EXCEPTION_NOT_HANDLED`    | Driver, hardware, or system issue      | Review referenced driver and recent changes               |
| `INACCESSIBLE_BOOT_DEVICE`       | Storage or boot configuration issue    | Check storage, drivers, and recent system changes         |
| BSOD after driver update         | Driver incompatibility                 | Investigate driver version and consider approved rollback |
| BSOD after hardware installation | Hardware or driver compatibility       | Remove or isolate recent hardware where appropriate       |

---

# Verification Checklist

* [ ] User symptoms documented
* [ ] Exact stop code recorded
* [ ] Time and frequency of crashes documented
* [ ] Recent software changes checked
* [ ] Recent driver changes checked
* [ ] Recent hardware changes checked
* [ ] Event Viewer reviewed
* [ ] Reliability Monitor reviewed
* [ ] Crash dump availability checked
* [ ] Windows system files checked
* [ ] Hardware health considered
* [ ] Memory diagnostics performed where appropriate
* [ ] Storage health considered
* [ ] Safe Mode tested where appropriate
* [ ] Windows Update history reviewed
* [ ] Security considerations evaluated
* [ ] User data protected before recovery actions
* [ ] Resolution applied
* [ ] System stability verified
* [ ] User confirms successful operation
* [ ] Ticket updated
* [ ] Escalation completed if required

---

# Escalation Criteria

Escalate the issue to the appropriate desktop support, systems, hardware, infrastructure, or security team when:

* BSODs occur repeatedly
* Hardware failure is suspected
* Memory or storage errors are detected
* Windows cannot boot normally
* System recovery is required
* A driver cannot be safely updated or rolled back
* Multiple computers experience the same BSOD
* A widespread software or driver issue is suspected
* System files remain corrupted after approved repair procedures
* A security compromise is suspected
* Data recovery may be required
* The issue requires administrative changes outside the support team's authorization

Provide the escalation team with:

* Device information
* Operating system version
* Stop code
* Referenced driver or file
* Date and time of crashes
* Crash frequency
* Recent changes
* Event Viewer findings
* Reliability Monitor findings
* Dump file information where available
* Hardware diagnostic results
* Troubleshooting performed
* Actions already taken
* User impact

---

# Documentation Requirements

Record the following information in the support ticket:

* Date and time reported
* Affected user
* Device or asset identifier according to organizational policy
* Operating system
* BSOD stop code
* Error message or referenced driver
* User activity immediately before the crash
* Recent hardware or software changes
* Diagnostic findings
* Troubleshooting performed
* Recovery actions
* Resolution
* Verification results
* Escalation details where applicable

Do not record passwords, recovery keys, authentication tokens, or other sensitive information.

---

# Prevention

Organizations can reduce recurring BSOD incidents by:

* Maintaining supported hardware configurations
* Keeping Windows updated
* Maintaining approved driver versions
* Testing drivers before broad deployment
* Monitoring endpoint health
* Maintaining reliable backup procedures
* Monitoring hardware health
* Replacing failing hardware promptly
* Controlling unauthorized software and drivers
* Maintaining endpoint security
* Documenting recurring stop codes and root causes
* Using controlled change management for system updates

---

# Related Runbooks

* [Slow Computer Troubleshooting](slow-computer-troubleshooting.md)
* [Laptop Setup Checklist](laptop-setup-checklist.md)
* [Incident Escalation](incident-escalation.md)
