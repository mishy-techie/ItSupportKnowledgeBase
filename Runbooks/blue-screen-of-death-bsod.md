# Blue Screen of Death (BSOD)

**Summary:** A Blue Screen of Death (BSOD), also called a stop error, is what Windows shows when it hits a critical problem and has to stop to protect itself. This runbook explains how to capture the evidence, narrow down the cause, restore stability, and know when to escalate.

**Environment:** Controlled IT support environment (Windows 10/11 computers, managed endpoints, Event Viewer, Windows recovery tools)

**Related runbooks:** `slow-computer-troubleshooting.md`, `laptop-setup-checklist.md`, `incident-escalation.md`

> **In plain English:** a BSOD is like a car's engine cutting out to prevent damage. The stop code is the warning light. It tells you where to start looking, but it rarely names the exact fault, so the job is to gather clues and work out the cause.

---

## Symptoms

A user may report:

- A blue screen showing a stop code (and, on newer Windows, a QR code)
- The computer restarting unexpectedly
- Crashes at startup, or only when using a certain app
- Crashes after a Windows update, driver install, or new hardware
- Crashes after waking from sleep
- A computer that cannot start Windows normally

Common stop codes:

| Stop code | Typical area |
|---|---|
| `MEMORY_MANAGEMENT` | RAM or memory |
| `PAGE_FAULT_IN_NONPAGED_AREA` | Driver, RAM, or system files |
| `IRQL_NOT_LESS_OR_EQUAL` / `DRIVER_IRQL_NOT_LESS_OR_EQUAL` | Drivers or memory |
| `SYSTEM_SERVICE_EXCEPTION` | Drivers, system files, or software |
| `KMODE_EXCEPTION_NOT_HANDLED` | Drivers, hardware, or system |
| `CRITICAL_PROCESS_DIED` | A critical Windows process failed |
| `INACCESSIBLE_BOOT_DEVICE` | Storage or boot configuration |

> **Note:** A stop code alone rarely proves the root cause. Always combine it with recent changes, logs, and dump files.

---

## Prerequisites

Before starting, make sure you have:

- **Authorization:** Permission to run diagnostics and, where needed, administrator rights on the device
- **Information from the user:** When it happened, what they were doing, whether it has happened before, and what changed recently
- **Tools:** Event Viewer, Reliability Monitor, Device Manager, Windows Memory Diagnostic, and an elevated Command Prompt or PowerShell
- **Approved sources:** Your organization's approved driver and update sources
- **Data protection plan:** Confirm backup or cloud sync status before any recovery action
- **BitLocker awareness:** On encrypted devices, recovery steps (such as Safe Mode or startup changes) may ask for a BitLocker recovery key. Retrieve it through your organization's approved process and never record it in the ticket

---

## Troubleshooting / Implementation

Work through these steps in order. Stop and escalate if you suspect hardware failure or a security issue.

### 1. Capture the incident
Record:

- Date and time, and whether it has happened before
- The exact **stop code** and any driver or file name shown
- What the user was doing at the time
- Whether the computer restarted by itself
- Whether it happens at startup or during normal use

If it appears again, photograph the screen (making sure no sensitive information is visible).

### 2. Check whether Windows can start
- **Starts normally:** continue with the steps below.
- **Crashes repeatedly at startup:** use Windows recovery options (Safe Mode, Startup Repair, System Restore, Windows Recovery Environment) and protect user data first. Do not reset or reimage yet.

### 3. Check for recent changes
Ask whether the first crash followed any of these:

- Windows update (quality, feature, or security)
- Driver or BIOS/firmware update
- New application or security software
- New hardware (RAM, SSD, dock, USB device, graphics, or network adapter)
- A configuration or policy change

A close match between a change and the first crash is a strong clue.

### 4. Review Event Viewer
1. Open **Event Viewer** and go to **Windows Logs > System**.
2. Look at events around the time of the crash.
3. Useful events include:
   - **BugCheck (Event ID 1001):** records the stop code after a crash
   - **Kernel-Power (Event ID 41):** the system restarted without a clean shutdown
   - **Event ID 6008:** the previous shutdown was unexpected

PowerShell example:

```powershell
Get-WinEvent -FilterHashtable @{LogName='System'; Id=1001,41,6008} -MaxEvents 20 |
  Select-Object TimeCreated, Id, ProviderName, Message
```

Not every error is the cause. Match timestamps with the crash.

### 5. Review Reliability Monitor
Open it by running:

```cmd
perfmon /rel
```

Look for failures near the crash time, and repeated failures from the same application or driver.

### 6. Check for crash dump files
Windows may save a dump file after a crash:

```text
C:\Windows\Minidump
C:\Windows\MEMORY.DMP
```

- Note the dates of the files and whether any exist.
- Dump files can be analyzed by the escalation team (for example, with Microsoft's WinDbg).
- Do not upload dump files to unapproved online services. They can contain sensitive data.

### 7. Check drivers
Drivers are a very common cause.

1. Open **Device Manager** and look for warning icons, missing devices, or recently changed devices.
2. Focus on graphics, network, storage controller, chipset, audio, docking station, USB, security, and virtualization drivers.
3. If a driver is suspected:
   - Identify the device and the current driver version
   - Check whether crashes began after an update
   - Install the **approved** driver, or roll back if approved
   - Restart and monitor stability

Do not install unapproved drivers or third-party driver-update tools.

### 8. Repair Windows system files
Open an **elevated** Command Prompt or PowerShell and run:

```cmd
sfc /scannow
```

If it finds problems it cannot fix, run:

```cmd
DISM /Online /Cleanup-Image /RestoreHealth
```

Then restart and run `sfc /scannow` again. Record whether files were found and repaired.

### 9. Check memory
Faulty RAM can cause `MEMORY_MANAGEMENT`, `PAGE_FAULT_IN_NONPAGED_AREA`, and random crashes.

1. Run the approved memory test (Windows Memory Diagnostic: search for `mdsched.exe`).
2. The test needs a restart. Save work first.
3. Check results in Event Viewer after restart. Memory errors mean the hardware needs escalating.

### 10. Check storage
Storage problems can cause crashes, corrupted files, and `INACCESSIBLE_BOOT_DEVICE`.

```powershell
Get-PhysicalDisk | Select-Object FriendlyName, MediaType, HealthStatus, OperationalStatus
```

```cmd
chkdsk C: /scan
```

If the disk shows warnings or errors, **protect the data first** and escalate.

### 11. Check recently added hardware
If new hardware was added, temporarily remove or isolate it if procedures allow, then see whether crashes stop.

### 12. Test Safe Mode
If the computer is stable in Safe Mode but crashes in normal mode, a third-party driver, startup app, or security software is a likely cause. Treat this as evidence, not proof.

### 13. Check for an application trigger
If only one app triggers the crash:

1. Make sure the app is fully updated and compatible with the Windows version.
2. Check its logs and related drivers.
3. Test whether the crash happens outside the app.
4. Escalate to the application owner if needed.

### 14. Review Windows Update history
If crashes began after an update, review **Windows Update history** and follow your approved rollback or remediation process. Do not remove security updates without authorization.

### 15. Consider security
Crashes can occasionally be linked to malware, unauthorized drivers, or endpoint-security conflicts. If anything looks suspicious:

1. Stop routine troubleshooting.
2. Preserve diagnostic information.
3. Follow your security incident procedure and escalate.

Do not disable endpoint protection just to test it.

### 16. Look for patterns in repeat crashes
For recurring BSODs, record the stop code, frequency, times, user activity, recent changes, driver versions, hardware, dump details, and Event Viewer findings. If several computers crash after the same update, driver, or policy change, treat it as a possible **wider incident** and escalate.

### When to escalate
Use `incident-escalation.md` when:

- Crashes keep happening after basic checks
- Hardware failure is suspected, or memory or storage errors are detected
- Windows cannot start normally, or recovery is required
- A driver cannot be safely updated or rolled back
- Several computers have the same BSOD
- System files stay corrupted after approved repairs
- A security compromise is suspected
- Data recovery may be needed
- The fix needs access outside your authority

**Include in the escalation:** device and OS version, stop code and referenced driver or file, dates and frequency, recent changes, Event Viewer and Reliability Monitor findings, dump file details, diagnostic results, what you already tried, and the user impact.

---

## Resolution

Choose the fix that matches the evidence you found.

| Finding | Typical resolution |
|---|---|
| Crash began after a driver update | Roll back or install the approved driver |
| Crash began after a Windows update | Follow the approved rollback or remediation process |
| Corrupted system files | `sfc /scannow` and DISM, then recheck |
| Memory errors | Escalate for RAM replacement or repair |
| Disk errors or health warnings | Back up data, then escalate for repair or replacement |
| New hardware suspected | Remove or replace the hardware (if permitted) |
| One app triggers it | Update the app, or escalate to the application owner |
| Suspected malware or compromise | Stop troubleshooting and escalate to security |
| No cause found, but system is stable | Monitor and document |

### Before any major recovery
- Confirm important data is backed up or synced
- Do not reimage or reset yet if the evidence is still needed to find the root cause
- Use System Restore, Startup Repair, or approved recovery steps only when authorized

### Document the ticket
Record:

- Date and time reported, user, and device or asset ID (per your organization's policy)
- Operating system version and stop code
- Referenced driver or error message
- User activity before the crash
- Recent changes and diagnostic findings
- Actions taken, recovery steps, and the resolution
- Verification results and any escalation

> **Never** record passwords, BitLocker recovery keys, tokens, or other secrets in the ticket.

---

## Verification

- [ ] Symptoms and exact stop code recorded
- [ ] Time and frequency of crashes documented
- [ ] Recent software, driver, and hardware changes checked
- [ ] Event Viewer and Reliability Monitor reviewed
- [ ] Crash dump availability checked
- [ ] System files checked (SFC and DISM)
- [ ] Memory and storage health checked where appropriate
- [ ] Safe Mode tested where appropriate
- [ ] Windows Update history reviewed
- [ ] Security considerations evaluated
- [ ] User data protected before recovery actions
- [ ] Fix applied
- [ ] Computer stable during normal use and after restart
- [ ] No new stop errors in Event Viewer after the fix
- [ ] User confirms normal operation
- [ ] Ticket updated, and escalation completed if required

Quick re-check for new crashes:

```powershell
Get-WinEvent -FilterHashtable @{LogName='System'; Id=1001,41} -MaxEvents 5 |
  Select-Object TimeCreated, Id, Message
```

> **In plain English:** the computer runs normally for a sensible period, there are no new crash events, and the user is happy it works.

---

## Common BSOD Scenarios

| Symptom | Possible cause | What to check |
|---|---|---|
| `MEMORY_MANAGEMENT` | RAM problem | Memory diagnostic and dump information |
| `PAGE_FAULT_IN_NONPAGED_AREA` | Driver, RAM, or system files | Recent drivers, memory, SFC |
| `IRQL_NOT_LESS_OR_EQUAL` | Driver or memory | Recent drivers and dump information |
| `SYSTEM_SERVICE_EXCEPTION` | Driver, system file, or software | Recent changes, drivers, SFC |
| `CRITICAL_PROCESS_DIED` | Critical Windows process failure | System files, drivers, crash information |
| `DRIVER_IRQL_NOT_LESS_OR_EQUAL` | Driver problem | Identify the referenced driver |
| `KMODE_EXCEPTION_NOT_HANDLED` | Driver, hardware, or system | Referenced driver and recent changes |
| `INACCESSIBLE_BOOT_DEVICE` | Storage or boot configuration | Disk health, storage drivers, recent changes |
| Crash after a driver update | Driver incompatibility | Driver version and approved rollback |
| Crash after new hardware | Hardware or driver compatibility | Remove or isolate the new hardware |

---

## Prevention

Organizations can reduce BSODs by:

- **Keeping Windows updated** and using controlled change management for updates
- **Using approved driver versions** and testing drivers on a small group before wide rollout
- **Maintaining supported hardware configurations** and replacing failing hardware promptly
- **Monitoring endpoint and hardware health** (disk health, memory errors, crash reports)
- **Controlling unauthorized software and drivers**
- **Keeping endpoint security up to date**
- **Maintaining reliable backups** so a crash never means lost data
- **Documenting recurring stop codes and root causes** so patterns are spotted quickly

---

## Lessons Learned

- **Capture evidence before restarting or changing things.** The stop code, timestamp, and dump files disappear or get overwritten.
- **Ask what changed.** A recent driver, update, or hardware change explains a large share of BSODs.
- **Drivers are the usual suspect.** Check them before assuming hardware has failed.
- **One crash is not a pattern, but repeated crashes are.** Track stop codes and dates.
- **Protect data first.** Do not reset or reimage until the user's data is safe and the evidence is no longer needed.
- **Several affected computers means a bigger problem.** It points to a shared update, driver, or policy.
- **Don't disable security tools to test.** Escalate instead.

### My notes
_Add your own lessons here from lab exercises or real tickets:_

- 

---

## References

- Microsoft Learn: Troubleshoot blue screen errors and bug check code reference
- Microsoft Learn: Read small memory dump files and analyze crashes with WinDbg
- Microsoft Learn: System File Checker (SFC) and DISM (Repair a Windows image)
- Microsoft Support: Windows Memory Diagnostic and Reliability Monitor
- Microsoft Learn: Event Viewer, including BugCheck (1001) and Kernel-Power (41) events
- Your organization's approved driver, update, backup, and incident response policies
