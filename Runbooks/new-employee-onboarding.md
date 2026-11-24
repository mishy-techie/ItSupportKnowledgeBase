# New Employee Onboarding

**Summary:** The IT side of onboarding a new starter: creating their accounts, assigning the right access, preparing their equipment, and testing everything so they can work from their first day.

**Environment:** Controlled IT support environment (identity provider, HR system, device management platform, productivity and collaboration tools, asset management system)

**Applies to:** New employees, contractors, interns, temporary staff, remote staff, and office-based staff

**Related runbooks:** `laptop-setup-checklist.md`, `employee-offboarding.md`, `account-lockout.md`, `password-reset.md`, `printer-troubleshooting.md`, `vpn-troubleshooting.md`, `wi-fi-connectivity.md`, `incident-escalation.md`

> **In plain English:** when someone joins, IT makes sure they have an account, the right access (and nothing extra), a working computer, and the apps they need, all tested before day one.

---

## Prerequisites

Before starting, make sure you have:

- **An approved request:** from HR or the hiring manager, through the organization's official process
- **Starter details:**
  - Name, start date, job title, department, and manager
  - Employment type (employee, contractor, intern, temporary)
  - Work location and whether office-based or remote
  - Required applications, access, hardware, and peripherals
  - Any special technical needs
- **Role access definition:** a documented list of access and software for the role (role template or access matrix)
- **Access to tools:** identity system, licensing portal, device management, and asset management
- **Available equipment:** laptop, accessories, and any required licenses
- **Enough lead time:** ideally start several days before the start date so testing can finish

> **Note:** Do not provision access without proper authorization. Never record passwords, MFA codes, or recovery keys in onboarding notes.

---

## Troubleshooting / Implementation

Follow these steps in order.

### 1. Confirm the request and the authorization
- [ ] Employee name, start date, job title, department, and manager
- [ ] Employment type and work location (office or remote)
- [ ] Required applications, access, hardware, and peripherals
- [ ] Approval from HR or the manager is on record

### 2. Define the access needed
Base access on the **role**, using **least privilege** and role-based access. Typical items:

- Email and productivity suite
- Collaboration tools and file storage
- Shared drives and departmental resources
- Business applications and internal websites
- VPN or remote desktop
- Printers and ticketing system

> **Warning:** Do not copy the permissions of an existing employee. They may hold extra access built up over time. Use the role template instead. Do not grant extra permissions for convenience.

### 3. Create the user account
Create the account through the approved identity process.

- [ ] Correct name, username, and email address
- [ ] Correct department and manager
- [ ] Correct organizational unit or groups
- [ ] Activation date set to the start date
- [ ] Required security policies applied

Example check (Active Directory, fictional user `jdoe`):

```powershell
Get-ADUser -Identity jdoe -Properties Enabled, Department, Manager, MemberOf |
  Select-Object Name, Enabled, Department, Manager, MemberOf
```

### 4. Set up authentication
- [ ] First-time password set-up process completed through the approved method
- [ ] MFA enrollment required
- [ ] Authentication methods configured
- [ ] Conditional access policies apply correctly (where used)
- [ ] Recovery procedure available

Give the employee their first sign-in details through the approved, secure process (for example, a one-time or temporary credential delivered separately from the username). Do not collect, store, or write down passwords or MFA codes.

### 5. Assign licenses
- [ ] Productivity suite and email
- [ ] Collaboration tools
- [ ] Business application licenses
- [ ] Security-related licenses
- [ ] Other role-specific software

Assign licenses **before** testing the related applications.

### 6. Prepare the computer
- [ ] Device selected and assigned to the employee
- [ ] Asset tag and serial number recorded
- [ ] Operating system, management enrollment, and security baseline done
- [ ] Windows updates installed

Follow `laptop-setup-checklist.md` for the full device preparation.

### 7. Prepare peripherals
- [ ] Monitor, keyboard, and mouse
- [ ] Headset and webcam
- [ ] Docking station and power adapter
- [ ] Laptop bag and other approved accessories

Record all issued equipment in the asset system.

### 8. Install and verify applications
Install role-specific software using **approved distribution methods**, such as:

- Office applications, browser, and PDF tools
- Collaboration and email applications
- VPN client and remote support software
- Security applications
- Business-specific applications

Then test each critical application, starting with those needed on day one:

- [ ] Application launches
- [ ] User can sign in
- [ ] License is active
- [ ] Required resources are reachable
- [ ] Permissions are correct

### 9. Set up email and collaboration
**Email**
- [ ] Mailbox exists and is reachable
- [ ] Calendar available
- [ ] Email policies apply
- [ ] Mobile or secondary access set up where authorized
- [ ] Approved test message sent

**Collaboration tools** (for example, Teams, Slack, or Zoom)
- [ ] Sign-in works
- [ ] Right teams or channels available
- [ ] Messaging, meetings, and audio/video work

### 10. Set up network access
**Office-based staff**
- [ ] Corporate Wi-Fi and wired network
- [ ] Internal applications and network drives
- [ ] Printers

If wireless problems occur, see `wi-fi-connectivity.md`.

**Remote staff or staff needing remote access**
- [ ] VPN client installed and profile configured
- [ ] Sign-in and MFA work
- [ ] Internal resources reachable
- [ ] Remote access policies applied

If VPN problems occur, see `vpn-troubleshooting.md`.

### 11. Set up file and data access
Confirm access to approved:

- Shared drives and SharePoint sites
- Cloud storage and departmental folders
- Business repositories and project resources

Access must match the role. Avoid broad access where limited access will do.

### 12. Set up printing (if required)
- [ ] Correct printer installed and selected
- [ ] Test page printed
- [ ] User has permission to print

If printing fails, see `printer-troubleshooting.md`.

### 13. Verify security controls
Do not bypass security to finish onboarding faster.

- [ ] Endpoint protection and firewall active
- [ ] Device encryption enabled
- [ ] Secure Boot enabled where required
- [ ] Device management active and compliant
- [ ] Security, screen lock, and update policies applied
- [ ] MFA enabled

### 14. Review access before the start date
Use this table to confirm each item is both **provisioned** and **tested**.

| Resource | Required | Provisioned | Tested |
|---|---|---|---|
| User account | Yes | [ ] | [ ] |
| Email | Yes | [ ] | [ ] |
| MFA | Yes | [ ] | [ ] |
| Productivity applications | Role-based | [ ] | [ ] |
| Collaboration tools | Role-based | [ ] | [ ] |
| VPN | Role-based | [ ] | [ ] |
| File storage | Role-based | [ ] | [ ] |
| Shared drives | Role-based | [ ] | [ ] |
| Business applications | Role-based | [ ] | [ ] |
| Printers | Role-based | [ ] | [ ] |

> **Note:** Test using a test account or the approved process, not by signing in with the new employee's credentials.

### 15. First-day support
On or before day one, confirm the employee can:

- [ ] Sign in to the computer and complete MFA
- [ ] Reach email and required applications
- [ ] Connect to the network
- [ ] Open required files

Give brief guidance, relevant to their environment, on:

- How to contact IT Support and report problems
- How to reset a password using approved methods
- How to connect to Wi-Fi and use the VPN
- How to reach company applications
- How to lock the computer and protect company information
- How to recognize and report suspicious activity

### 16. Update asset records
Record in the asset system:

- Asset tag, serial number, device type, and model
- Assigned employee and department
- Assignment date and accessories issued
- Device status

Make sure the record matches the physical equipment handed over.

### Common onboarding issues

| Issue | Possible cause | What to check |
|---|---|---|
| User cannot sign in | Account, password, or authentication problem | Account status and authentication setup |
| MFA cannot be completed | Enrollment problem | Approved MFA registration process |
| Email unavailable | License or mailbox not provisioned | License assignment and mailbox status |
| Application unavailable | Installation, license, or permissions | Installation, license, and access |
| VPN unavailable | Network, profile, or authentication | `vpn-troubleshooting.md` |
| Wi-Fi unavailable | Adapter, authentication, or network | `wi-fi-connectivity.md` |
| Shared drive unavailable | Permissions, network, or VPN | Access requirements and connectivity |
| Printer unavailable | Configuration or network | `printer-troubleshooting.md` |
| Device not compliant | Policy or enrollment issue | Device management and security policies |
| Laptop not ready | Setup or deployment issue | `laptop-setup-checklist.md` |
| New account not appearing in a cloud service | Identity sync delay or failure | Wait for sync, then check sync status and escalate |

### When to escalate
Use `incident-escalation.md` when:

- Account creation or identity synchronization fails
- Licenses cannot be assigned
- Application access cannot be provisioned
- Device enrollment fails, or hardware is unavailable or defective
- Security policies cannot be applied
- Network access cannot be established
- Access needs specialist approval, or the role's access is unclear
- The request exceeds standard role-based permissions
- The fix needs privileges beyond your authority

**Include in the escalation:** employee role, start date, affected system, access requested, error messages, what you tried, current provisioning status, business impact, and the date it must be ready.

---

## Resolution

The onboarding request is resolved when every step is complete, the Verification checklist is ticked, and:

1. The manager and HR are told that IT onboarding is ready.
2. The employee has received their equipment and first sign-in details through approved channels.
3. Any open items are assigned an owner and a date.

### Document the request
Record:

- Employee name, start date, department, manager, and role
- Assigned hardware and asset information
- Accounts created and licenses assigned
- Applications installed and access granted
- Testing completed
- Outstanding issues and escalations
- Completion status

> **Never** record passwords, MFA codes, tokens, or recovery keys in the ticket.

---

## Verification

The IT onboarding request is complete only when all of these are confirmed.

### Account and access
- [ ] User account active
- [ ] Correct licenses assigned
- [ ] MFA configured
- [ ] Access matches the role and approval on record

### Device and security
- [ ] Laptop or desktop prepared and assigned
- [ ] Device management enrollment complete
- [ ] Security controls active
- [ ] Required peripherals available

### Applications and connectivity
- [ ] Required applications installed
- [ ] Email and collaboration tools working
- [ ] Network access working
- [ ] VPN working where required
- [ ] File access working
- [ ] Printers configured where required

### Wrap-up
- [ ] Asset records updated
- [ ] User received required IT guidance
- [ ] Documentation complete
- [ ] No unresolved critical onboarding issues

> **In plain English:** the new starter can sign in on day one, reach everything they need and nothing they shouldn't, and we have a record of what they were given.

---

## Prevention

Most onboarding problems come from late requests, unclear access needs, and manual steps. These practices reduce them.

### Plan ahead
- **Set onboarding lead times** so IT gets notice early enough to test
- **Define onboarding service targets** (for example, ready two working days before the start date)
- **Test access before the start date**, not on day one

### Standardize
- **Use a standard onboarding checklist** every time
- **Document applications and access by role**
- **Use standard laptop configurations**
- **Use role-based access groups** instead of one-off permissions

### Automate
- **Automate account provisioning** from the HR system where possible
- **Automate device enrollment and software deployment**

### Keep records and improve
- **Keep asset records accurate**
- **Review recurring onboarding failures** and fix the cause
- **Collect feedback** from new starters and managers

---

## Lessons Learned

- **Start early.** Most day-one problems are caused by late requests.
- **Define access by role.** Copying a colleague's access can pass on unnecessary permissions.
- **"Created" is not "tested."** Check that each item works, not just that it exists.
- **License before testing.** A missing license looks like a broken application.
- **Allow for sync delays.** New accounts can take time to appear in cloud services.
- **Keep credentials out of tickets.** Use secure, approved delivery for first sign-in details.
- **Match records to reality.** The asset record should match what the employee actually received. This helps later at offboarding.
- **A good first day builds trust in IT.** Be ready, and make support easy to find.

### My notes
_Add your own lessons here from lab exercises or real onboarding:_

- 

---

## References

- Microsoft Learn: Microsoft Entra ID and Microsoft 365 user and license management
- Microsoft Learn: Active Directory user account management (if applicable)
- Microsoft Learn: Windows Autopilot and Intune device enrollment
- NIST SP 800-53, Access Control family (least privilege, account management)
- CIS Critical Security Controls: Account Management and Access Control Management
- Your organization's onboarding, access control, security, and asset management policies
