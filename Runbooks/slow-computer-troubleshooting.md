# Slow Computer Troubleshooting

**Summary:** "My computer is slow" can mean many things. This runbook helps you pin down what is slow, find the bottleneck (processor, memory, disk, startup, network, application, or hardware), apply a fix, and confirm the computer is faster.

**Environment:** Controlled IT support environment (Windows 10/11 desktops and laptops, business applications, endpoint management and security software)

**Related runbooks:** `wi-fi-connectivity.md`, `vpn-troubleshooting.md`, `blue-screen-of-death-bsod.md`, `laptop-setup-checklist.md`, `incident-escalation.md`

> **In plain English:** a computer has four main "workers": the processor (CPU), memory (RAM), storage (disk), and the network. If any one is overloaded, everything feels slow. This runbook finds which one is struggling and why.

---

## Symptoms

A user may report:

- Slow start-up or slow Windows sign-in
- Applications are slow to open or freeze ("Not Responding")
- Typing or mouse movement is delayed
- Files take a long time to open
- Browser is slow
- The computer gets slower the longer it runs
- Fan runs constantly, or the computer feels hot
- Disk usage stays high
- It is only slow on Wi-Fi, over VPN, or when using one application
- Performance has declined gradually over time

---

## Prerequisites

Before starting, make sure you have:

- **Information from the user:** what exactly is slow, when it began, how often, which applications, and what changed recently
- **Access:** local administrator rights on the computer for diagnostics (where authorized)
- **Tools:** Task Manager, Resource Monitor, Event Viewer, Reliability Monitor, and an elevated Command Prompt or PowerShell
- **Backup status:** confirm important data is backed up before any cleanup or repair
- **Hardware details:** the device's specifications, age, and the user's normal workload (from the asset record)

---

## Troubleshooting / Implementation

Work through these steps in order. Note what you see at each stage.

### 1. Define "slow"
Ask and record:

- What is slow: the whole computer, one application, start-up, sign-in, file access, or the internet?
- When did it start, and is it constant or intermittent?
- Does it happen after start-up or only after hours of use?
- Is it slow only on the network, on Wi-Fi, or over VPN?
- Was new software or hardware added, or Windows updated, recently?
- Has the user's workload changed (bigger files, more applications)?

### 2. Check the scope
| Scope | Likely areas |
|---|---|
| **One computer** | Hardware limits, local software, background processes, storage, Windows configuration, malware, corrupted system files |
| **Many computers** | Network, shared applications, servers, cloud service issues, a software deployment, security policy changes |

If several users report the same problem, treat it as a possible wider incident and escalate (see **When to escalate**).

### 3. Restart the computer
Check how long it has been running:

```powershell
(Get-Date) - (Get-CimInstance Win32_OperatingSystem).LastBootUpTime
```

A very long uptime can build up hung processes, memory use, and pending updates. Do a proper **Restart** (not Shut down, because Windows "Fast startup" can keep parts of the old session). If the problem returns, keep going. A restart is a workaround, not a fix.

### 4. Check Task Manager
Open **Task Manager** (Ctrl+Shift+Esc) and review **CPU, Memory, Disk, Network, and Startup apps**.

- Match what you see with the user's symptoms before ending any process.
- Do not end unknown, system, or security processes unless you understand them and are authorized.

Quick command-line view:

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10 Name, CPU, WorkingSet
```

### 5. Check CPU usage
High CPU can come from heavy applications, Windows updates, antivirus scans, browsers, software installs, application errors, malware, or hardware limits.

Work out whether it is:

- Short-lived or constant
- Tied to one application or to system activity

If one application is using a lot of CPU:

1. Confirm the application is needed and whether it is doing a known heavy task.
2. Check for application updates.
3. Restart the application and see whether it returns.
4. Review the application's logs where available.

If a Windows process is responsible, investigate before ending it.

### 6. Check memory
High memory use causes slowdowns, excess paging, freezes, and crashes. Common causes are web browsers (many tabs or extensions), large spreadsheets, virtual machines, development tools, and collaboration apps.

```powershell
Get-CimInstance Win32_OperatingSystem | Select-Object @{n='TotalGB';e={[math]::Round($_.TotalVisibleMemorySize/1MB,1)}}, @{n='FreeGB';e={[math]::Round($_.FreePhysicalMemory/1MB,1)}}
```

If memory stays consistently high:

1. Identify which applications use it.
2. Close unnecessary applications.
3. Restart the affected application or the computer.
4. Decide whether the computer has enough RAM for the user's workload.

Repeated high memory use may mean the workload has outgrown the device.

### 7. Check disk activity
Constant high disk use can come from Windows Update, antivirus scans, file synchronization (for example, cloud storage), search indexing, low memory, failing storage, or background processes.

1. Find the process causing the activity (Task Manager or **Resource Monitor**: run `resmon`).
2. Check whether it is temporary (for example, an update or scan) or ongoing.
3. Check what type of drive the computer has:

```powershell
Get-PhysicalDisk | Select-Object FriendlyName, MediaType, HealthStatus, OperationalStatus
```

Older mechanical hard drives (HDD) are much slower than solid-state drives (SSD) and are a common cause of "slow" computers.

### 8. Check free disk space
Low free space slows Windows and applications and can stop updates installing.

```powershell
Get-PSDrive C | Select-Object @{n='UsedGB';e={[math]::Round($_.Used/1GB,1)}}, @{n='FreeGB';e={[math]::Round($_.Free/1GB,1)}}
```

Look at temporary files, the Recycle Bin, Downloads, large user files, application caches, Windows update files, and local sync folders. Windows **Storage settings** or Disk Cleanup can help.

> **Warning:** Do not delete business data or system files unless you know what they are and have confirmed with the user or your policy.

### 9. Check storage health
Signs of a failing drive include very slow file operations, frequent freezes, disk errors, files that take a long time to open, and repeated crashes.

```cmd
chkdsk C: /scan
```

If you suspect storage failure, **protect the user's data first** and escalate. Run hardware diagnostics only through your approved procedure.

### 10. Review start-up programs
Too many start-up programs slow boot and sign-in. In **Task Manager > Startup apps**, look for unnecessary or resource-heavy items.

```powershell
Get-CimInstance Win32_StartupCommand | Select-Object Name, Command, Location
```

Disable only what is appropriate and permitted by policy. Do **not** disable security, management, or business-critical applications without authorization.

To see how long recent boots took, check Event Viewer under **Applications and Services Logs > Microsoft > Windows > Diagnostics-Performance > Operational** (Event ID 100 records boot time).

### 11. Investigate slow sign-in
If it is slow mainly at sign-in, check:

- Group Policy processing and sign-in scripts
- Network connectivity and mapped drives
- Roaming or redirected profiles (where used)
- Authentication services
- Start-up applications
- Endpoint management and security agents

If several users have slow sign-ins, investigate domain or infrastructure causes and escalate.

### 12. Check whether one application is the problem
If only one application is slow:

1. Test another application and see whether Windows itself responds well.
2. Restart the application and check for updates.
3. Review its resource use and configuration (for example, add-ins or browser extensions).
4. Check network connectivity if the application depends on the network.
5. Test with another user profile if authorized.

If other applications run normally, focus on the affected application, not the whole computer.

### 13. Check whether the network is the cause
Some applications seem slow because they rely on the network (cloud apps, file shares, remote desktops, VPN, web-based apps). Test the basics:

```cmd
ping <default-gateway>
nslookup example.com
```

If it is only slow when using a network resource, investigate the network path, not the computer.

- Wireless: see `wi-fi-connectivity.md`
- VPN: see `vpn-troubleshooting.md`

### 14. Check power settings
A "Power saver" plan can reduce performance, especially on laptops.

```cmd
powercfg /getactivescheme
```

Change the power plan only if allowed by your organization's policy.

### 15. Check Windows updates
Pending or recently installed updates can affect performance.

- Check pending, recent, and failed updates, and whether a restart is needed.
- If slowness began right after an update, record the timing and investigate.
- Do not remove security updates without authorization.

### 16. Check Windows system files
If Windows seems unstable or damaged, run in an elevated window:

```cmd
sfc /scannow
DISM /Online /Cleanup-Image /RestoreHealth
```

Let each finish before judging the result, and note the outcome in the ticket.

### 17. Consider security
Slowness can be caused by malware, suspicious processes, unauthorized software, cryptomining, or unusual network activity.

- Follow your endpoint security procedures.
- **Do not** turn off antivirus or endpoint protection just to see whether it is the cause.
- If you suspect malicious activity, stop routine troubleshooting and follow your security incident process.

### 18. Check hardware limits
Compare the computer's specifications with the user's workload (RAM, CPU, storage type and capacity, device age, thermal condition, hardware health).

```powershell
Get-ComputerInfo | Select-Object CsName, OsName, CsProcessors, CsTotalPhysicalMemory
```

A computer can be healthy and still be too small for the job. Examples include too little RAM for large applications, an old HDD, a slow CPU, or too little storage. Upgrades and replacements should follow asset management procedures.

### 19. Check for overheating
Signs: loud or constant fans, slowdown after long use, unexpected shutdowns, high temperatures, or reduced speed under load (thermal throttling).

Check for blocked vents, dust, dock or power problems, the environment, and hardware diagnostic warnings. Internal cleaning or repairs should follow approved procedures and manufacturer guidance.

### 20. Check Event Viewer
Look at **Windows Logs > System** and **Application** near the time of the slowdown for:

- Disk errors
- Application errors
- Service failures
- Driver problems
- Unexpected shutdowns
- Hardware warnings

Not every warning is the cause. Match timestamps with the user's symptoms.

```powershell
Get-WinEvent -FilterHashtable @{LogName='System'; Level=1,2; StartTime=(Get-Date).AddDays(-1)} -MaxEvents 20 |
  Select-Object TimeCreated, Id, ProviderName, Message
```

### 21. Check Reliability Monitor
```cmd
perfmon /rel
```

Look for application crashes, driver or hardware failures, and update failures that line up with when the slowness began. This is especially useful for intermittent problems.

### When to escalate
Use `incident-escalation.md` when:

- Hardware or storage failure is suspected
- Several users have the same problem
- A shared application is affected
- Network infrastructure seems responsible
- The computer keeps crashing (see `blue-screen-of-death-bsod.md`)
- System files cannot be repaired
- Malware or unauthorized activity is suspected
- Performance is still poor after approved troubleshooting
- A hardware upgrade or replacement is needed
- The fix needs privileges beyond your authority

**Include in the escalation:** device and OS details, symptoms and start time, CPU, memory, and disk findings, applications affected, error messages, Event Viewer or Reliability Monitor findings, what you tried and the results, and how many users are affected.

### Common slow computer scenarios

| Symptom | Possible cause | What to check |
|---|---|---|
| Slow right after start-up | Start-up apps or updates | Start-up programs and Windows Update |
| Applications slow | CPU, memory, disk, or application issue | Task Manager and application behavior |
| Disk at 100% | Background process, failing drive, low memory | Process using the disk; storage health |
| Memory constantly high | Too many applications or too little RAM | Memory-heavy processes and workload |
| Slower the longer it runs | Memory leak, background process, overheating | Restart, then monitor and look for repeating processes |
| One application slow | Application-specific problem | Test other applications and examine the application |
| Files open slowly | Storage or network issue | Whether files are local or on the network |
| Slow sign-in | Group Policy, scripts, network, or profile | Sign-in dependencies |
| Slow only on Wi-Fi | Wireless problem | `wi-fi-connectivity.md` |
| Slow only over VPN | VPN or network performance | `vpn-troubleshooting.md` |
| Fan runs constantly | High CPU, overheating, or background activity | Processes and hardware conditions |
| Slow after an update | Update or driver issue | Recent updates and timing |

---

## Resolution

Apply the fix that matches what you found:

| Finding | Typical resolution |
|---|---|
| Long uptime or hung processes | Restart the computer |
| Heavy or misbehaving application | Update, restart, reconfigure, or escalate to the application owner |
| Too many start-up programs | Disable unnecessary ones (if permitted) |
| Low disk space | Clear temporary files, move or archive data (with approval) |
| Corrupted system files | `sfc /scannow` and DISM |
| Pending or failed updates | Complete or repair updates |
| Power saver plan | Change to a balanced plan (if permitted) |
| Insufficient RAM, old HDD, or old device | Request a hardware upgrade or replacement |
| Overheating | Clear vents, check dock and power, escalate for repair |
| Suspected malware | Stop and escalate to security |
| Network-related | Use the Wi-Fi or VPN runbook, or escalate to the network team |
| Failing storage | Protect data, then escalate |

### Document the ticket
Record:

- Date and time reported, user, and device or asset ID
- Operating system and symptoms
- Applications affected
- CPU, memory, disk, and storage findings
- Network findings and error messages
- Troubleshooting performed and changes made
- Resolution and verification results
- Escalation details, if any

> **Never** record passwords, tokens, recovery keys, or other sensitive information in the ticket.

---

## Verification

- [ ] User symptoms documented
- [ ] Scope determined
- [ ] Computer restarted where appropriate
- [ ] CPU, memory, and disk usage checked
- [ ] Free storage checked
- [ ] Start-up programs reviewed
- [ ] Application-specific performance checked
- [ ] Network dependency evaluated
- [ ] Windows Update status checked
- [ ] System integrity checked where appropriate
- [ ] Event Viewer and Reliability Monitor reviewed where appropriate
- [ ] Security and hardware considerations evaluated
- [ ] Resolution applied
- [ ] Performance re-tested (boot time, application launch, normal workload)
- [ ] Performance remains good after a restart and some time of use
- [ ] User confirms improvement
- [ ] Ticket updated, and escalation completed if required

> **In plain English:** the computer starts and runs the user's normal work at a good speed, and it stays that way after a restart and a few hours of use.

---

## Prevention

Organizations can reduce slow-computer problems by:

- **Setting hardware standards** and using SSDs and enough RAM for each role
- **Replacing aging devices** on a planned cycle
- **Keeping Windows, drivers, and approved applications updated**
- **Monitoring endpoint health** (disk space, memory, crashes, boot times)
- **Keeping enough free disk space**, with alerts for low space
- **Reviewing and limiting start-up applications**
- **Using endpoint management policies** to control software and settings
- **Keeping endpoint security up to date** and scheduling scans outside working hours
- **Tracking recurring performance problems** by application, model, or team
- **Educating users** on safe software installation, closing unused applications, and restarting regularly
- **Documenting root causes** of repeated issues so they can be fixed at source

---

## Lessons Learned

- **Ask what "slow" means.** Slow start-up, slow browser, and slow file access are three different problems.
- **Measure before changing things.** Task Manager shows whether CPU, memory, disk, or network is the bottleneck.
- **One computer or many?** If many, it is probably not the computers.
- **Disk activity is often the hidden cause.** An HDD, a full drive, or a sync or scan can make a powerful computer feel slow.
- **Restart properly.** "Shut down" and "Restart" are not the same on a system with Fast startup.
- **Don't disable security tools to test them.** Use the approved process.
- **Healthy is not the same as suitable.** A computer with no faults can still be too small for the workload.
- **Correlate, don't guess.** An Event Viewer warning only matters if it lines up with the symptom.

### My notes
_Add your own lessons here from lab exercises or real tickets:_

- 

---

## References

- Microsoft Learn: Troubleshoot Windows performance issues, Task Manager, and Resource Monitor
- Microsoft Learn: System File Checker (SFC) and DISM
- Microsoft Learn: `Get-PhysicalDisk`, `Get-CimInstance`, `Get-Process`, and `Get-WinEvent`
- Microsoft Support: Reliability Monitor, Storage settings, and power plans (`powercfg`)
- Microsoft Learn: Windows Diagnostics-Performance event logs (boot performance)
- Your organization's hardware standards, endpoint security, and incident response procedures
