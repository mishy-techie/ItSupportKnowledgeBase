# Laptop Setup Checklist

**Summary:** A repeatable process for preparing a Windows laptop before it goes to a user, so every device is secure, up to date, managed, tested, and recorded the same way.

**Environment:** Controlled IT support environment (Windows 10/11 business laptops, device management platform such as Microsoft Intune or similar, identity provider, asset management system)

**Applies to:** New, replacement, reassigned, and refurbished company laptops

**Related runbooks:** `new-employee-onboarding.md`, `employee-offboarding.md`, `wi-fi-connectivity.md`, `vpn-troubleshooting.md`, `slow-computer-troubleshooting.md`, `blue-screen-of-death-bsod.md`, `account-lockout.md`, `password-reset.md`, `incident-escalation.md`

> **In plain English:** this is the "pre-flight check" for a laptop. Before someone gets it, we confirm it works, is protected, has the right software, and is written down in our records.

---

## Prerequisites

Before starting, make sure you have:

- **The device:** laptop, power adapter, and any dock or accessories
- **Assignment details:** user name, department, role, required applications, and permissions (from the onboarding or service request)
- **Access:** rights to your device management platform, identity system, and asset management system
- **Approved sources:** your organization's approved images, drivers, and software packages
- **A reliable network connection** for provisioning and updates
- **Policies at hand:** naming convention, encryption, data handling, and security baseline

> **Note:** Never write passwords, MFA codes, or BitLocker recovery keys in setup notes or tickets. Use approved processes only.

---

## Troubleshooting / Implementation

Work through the phases in order. Tick items as you go.

### Phase 1: Confirm the device and the user
- [ ] Make and model, serial number, and asset tag recorded
- [ ] Power adapter, dock, and accessories present
- [ ] Device name assigned (if already allocated)
- [ ] User name, department, role, and device assignment confirmed
- [ ] Required applications, hardware, and permissions identified

Check the serial number against the asset record:

```powershell
Get-CimInstance Win32_BIOS | Select-Object SerialNumber
```

### Phase 2: Inspect the hardware
- [ ] Screen, keyboard, touchpad, and casing
- [ ] Hinges and power button
- [ ] USB, USB-C, HDMI/display, audio, and charging ports
- [ ] Webcam, microphone, and speakers
- [ ] Wireless adapter present

Document any damage **before** assigning the device.

### Phase 3: Check power and battery
- [ ] AC adapter works and laptop charges
- [ ] Battery is detected, healthy, and runs the laptop on battery power
- [ ] Charging indicator works

Generate a battery health report:

```cmd
powercfg /batteryreport
```

Escalate battery or charging faults before deployment.

### Phase 4: Install or reset Windows
1. Start Windows setup and choose language, region, and keyboard.
2. Connect to an approved network.
3. Apply your organization's provisioning process (for example, Windows Autopilot or an approved image).
4. For reassigned devices, use the approved reset, wipe, or reimage process and make sure previous user data is removed per policy.

### Phase 5: Name and enroll the device
- [ ] Name follows the naming standard and is unique
- [ ] Name is recorded in the asset system
- [ ] Device is enrolled in the management platform
- [ ] Device appears in the console with the correct user
- [ ] Policies and configuration profiles applied
- [ ] Compliance status reports correctly

Allow time for policies and apps to sync. To check join and enrollment status:

```cmd
dsregcmd /status
```

### Phase 6: Update Windows and drivers
- [ ] Approved Windows updates installed
- [ ] Required security and driver updates installed
- [ ] Restarts completed, then rechecked for more updates
- [ ] **Device Manager** shows no warnings or unknown devices

Check drivers for chipset, graphics, network, Wi-Fi, Bluetooth, audio, storage, camera, touchpad, and dock. Use only approved driver sources.

### Phase 7: Configure the user account
- [ ] Correct user account and device-user association
- [ ] Authentication method set up
- [ ] MFA enrollment completed where required
- [ ] Correct organizational access

Do not handle or store user passwords outside approved authentication processes.

### Phase 8: Confirm security controls
Do **not** disable security controls to speed up deployment.

- [ ] Antivirus or endpoint protection active
- [ ] Firewall enabled
- [ ] Endpoint detection and response (EDR) active, where used
- [ ] Device encryption enabled (BitLocker or required solution)
- [ ] Recovery information stored according to policy
- [ ] Secure Boot enabled where required
- [ ] Screen lock and security policies applied
- [ ] No unnecessary local administrator access

Useful checks:

```powershell
Get-BitLockerVolume
Get-Tpm
Confirm-SecureBootUEFI
Get-NetFirewallProfile | Select-Object Name, Enabled
Get-MpComputerStatus | Select-Object AntivirusEnabled, RealTimeProtectionEnabled
net localgroup administrators
```

> **Note:** `Get-MpComputerStatus` reports Microsoft Defender only. If your organization uses another endpoint protection product, check it in that product's console.

### Phase 9: Install and license applications
Install role-specific software from **approved sources only**, such as:

- Office productivity, browser, collaboration, and PDF tools
- VPN client and remote support tools
- Security software
- Business-specific applications

Then confirm:

- [ ] Applications are licensed and activate correctly
- [ ] The user has the right permissions
- [ ] Business applications are reachable

Do not use personal accounts or unauthorized licenses. To check Windows activation:

```cmd
slmgr /xpr
```

### Phase 10: Test networking
**Wi-Fi**
- [ ] Adapter works and correct SSID is visible
- [ ] Authentication works and an IP address is received
- [ ] Internet and internal resources are reachable

**Ethernet and docking station** (if used)
- [ ] Ethernet, external displays, and USB devices work
- [ ] Laptop charges through the dock where supported
- [ ] Network switches correctly when docking and undocking

**VPN** (if remote access is required)
- [ ] Approved client and profile installed
- [ ] Sign-in and MFA work
- [ ] VPN connects and internal resources are reachable

Quick connectivity check:

```cmd
ipconfig /all
ping 8.8.8.8
```

> **Note:** If a connection fails, use `wi-fi-connectivity.md` or `vpn-troubleshooting.md`.

### Phase 11: Apply user environment settings
Apply **approved** organizational settings only:

- Desktop and Start menu configuration
- Browser settings and corporate bookmarks
- Default applications and email setup
- File synchronization and collaboration tools
- Network drive mappings where applicable

Avoid unnecessary personal customization during standard setup.

### Phase 12: Migrate data (replacement or reassigned devices)
- [ ] Required files backed up
- [ ] Approved cloud sync configured
- [ ] Required business data transferred
- [ ] Previous device data handled securely (and wiped per policy)
- [ ] Transfer completed successfully

Never copy sensitive business data to unauthorized storage.

### Phase 13: Test peripherals
- [ ] External monitor, keyboard, mouse, and headset
- [ ] Webcam and microphone (test audio in and out for video calls)
- [ ] Docking station
- [ ] Printer, where required

### Phase 14: Run functional and health checks
Test as the user would use it:

- [ ] Windows sign-in and user account
- [ ] Internet, Wi-Fi, and VPN
- [ ] Required applications, email, and collaboration tools
- [ ] File access and internal resources
- [ ] Audio, microphone, webcam, and external display

System health:

- [ ] CPU, memory, and storage recognized correctly
- [ ] Enough free disk space
- [ ] No unresolved Device Manager errors or critical Windows errors
- [ ] Battery works normally and temperature looks normal
- [ ] No unexpected performance problems

```powershell
Get-ComputerInfo | Select-Object CsName, OsName, OsVersion, CsTotalPhysicalMemory
Get-PhysicalDisk | Select-Object FriendlyName, HealthStatus
Get-PSDrive C
```

### Phase 15: Update asset records
Record in the asset system:

- Asset tag, serial number, and model
- Assigned user and department
- Device name and operating system
- Deployment date
- Warranty information, where applicable
- Accessories issued and device status

### Common setup problems

| Problem | Likely cause | What to do |
|---|---|---|
| Device does not appear in the management console | Enrollment incomplete or no network | Check connectivity, then re-run enrollment |
| Policies or apps not applying | Sync not finished | Wait, force a sync, and recheck compliance |
| Windows will not activate | Licensing or network issue | Check licensing and connectivity, then escalate |
| BitLocker will not enable | TPM disabled or policy conflict | Check TPM and BIOS settings, then escalate |
| Software will not install | Missing license or permissions | Check licensing and approved source, then escalate |
| Wi-Fi or VPN fails | Profile, certificate, or credentials | Follow the related runbook |
| Unknown device in Device Manager | Missing driver | Install the approved driver |
| Laptop crashes during setup | Driver or hardware fault | Follow `blue-screen-of-death-bsod.md` |
| Setup is slow or sluggish | Updates running, low resources | Wait for updates, then see `slow-computer-troubleshooting.md` |

### When to escalate
Use `incident-escalation.md` when:

- Hardware is defective or diagnostics fail
- Device enrollment fails
- Security policies cannot be applied
- Windows activation fails
- Required applications cannot be installed or licensed
- Network access cannot be established
- VPN setup needs administrative changes
- Encryption cannot be enabled
- The device does not meet security requirements
- The fix needs permissions beyond your authority

Before escalating, record the problem, what you tried, error messages, and the device's current status.

---

## Resolution

A laptop is ready for handover once all setup phases are complete and the Verification checklist is fully ticked.

### Prepare for handover
- [ ] Laptop cleaned
- [ ] Power adapter and accessories included
- [ ] User account verified
- [ ] Applications installed and network access tested
- [ ] VPN tested where required
- [ ] Security controls verified
- [ ] Asset record updated

### Brief the user
Explain, at a level suited to their role:

- How to sign in and lock the device when unattended
- How to connect to Wi-Fi and use the VPN
- How to reach business applications
- How to contact IT Support and report problems
- How to protect company information and handle company equipment

### Close the request
1. Record the handover date and the user's acknowledgment (per your organization's process).
2. Update the ticket with what was set up, any exceptions, and any outstanding items.
3. Close the request only when no critical issues remain.

---

## Verification

Final deployment checklist. All items must be confirmed before handover.

### Device and records
- [ ] Hardware inspected and any damage documented
- [ ] Asset information recorded and records updated
- [ ] User assignment confirmed
- [ ] Device name configured and unique

### Software and management
- [ ] Windows installed or reset
- [ ] Device enrolled and compliant in management
- [ ] Windows updates and drivers verified
- [ ] Required applications installed and licensed
- [ ] User account and MFA configured

### Security baseline
- [ ] Endpoint protection active
- [ ] Firewall enabled
- [ ] Device encryption enabled and recovery info stored per policy
- [ ] Secure Boot enabled where required
- [ ] Security policies applied
- [ ] No unnecessary local administrator access
- [ ] No unauthorized software installed

### Connectivity and function
- [ ] Wi-Fi tested
- [ ] Ethernet and docking station tested where required
- [ ] VPN tested where required
- [ ] Peripherals tested
- [ ] User data migrated where required
- [ ] Functional testing completed
- [ ] System health checked

### Handover
- [ ] Accessories confirmed
- [ ] User briefed
- [ ] Handover completed and documented

> **In plain English:** the laptop is protected, working, and written into our records, and the user knows how to use it and where to get help.

---

## Prevention

Most setup problems come from manual steps, inconsistent images, and missed checks. These practices reduce them.

### Standardize
- **Use a standard, regularly updated image or provisioning profile** (for example, Windows Autopilot)
- **Use one approved checklist** for every device
- **Enforce the naming convention** automatically where possible

### Automate
- **Automate enrollment, policies, and application deployment** through your management platform
- **Automate compliance reporting** so security gaps show up straight away
- **Pre-stage common drivers and software** to cut setup time

### Keep records
- **Keep asset records accurate** and updated at every handover
- **Track warranty and lifecycle dates**
- **Record recurring setup issues** and fix them in the image or process

### Control risk
- **Never disable security controls** to save time
- **Limit local administrator access**
- **Review the checklist regularly** to remove outdated steps and add new ones

---

## Lessons Learned

- **Inspect before you configure.** Finding a cracked screen or failing battery after setup wastes time and causes disputes.
- **Test as the user, not just as IT.** A laptop can work for you and still fail for the user's role, permissions, or apps.
- **Check for a second round of updates.** One restart often reveals more updates.
- **Allow time for management policies to sync.** Many "failed" setups are just unfinished syncs.
- **Don't skip encryption or security checks.** Fixing them after handover is harder and riskier.
- **Record the device straight away.** Missing asset records cause problems later, especially at offboarding.
- **Standard beats custom.** The more a laptop matches the standard build, the easier it is to support.

### My notes
_Add your own lessons here from lab builds or real deployments:_

- 

---

## References

- Microsoft Learn: Windows Autopilot and Microsoft Intune device enrollment
- Microsoft Learn: BitLocker overview and deployment
- Microsoft Learn: Windows security baselines and Secure Boot
- Microsoft Learn: PowerShell cmdlets for `Get-BitLockerVolume`, `Get-Tpm`, `Get-NetFirewallProfile`, and `Get-MpComputerStatus`
- Microsoft Support: `powercfg` battery report and Windows activation
- Your organization's naming convention, security baseline, data handling, and asset management policies
