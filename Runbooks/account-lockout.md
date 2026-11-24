# Account Lockout

**Summary:** An account lockout happens when a user account is temporarily blocked after too many failed sign-in attempts. This runbook explains how to find out why it happened, safely restore access, and stop it from happening again.

**Environment:** Controlled IT support environment (Active Directory Domain Services, Windows domain-joined computers, Microsoft 365 / Microsoft Entra ID where applicable)

**Related runbooks:** `password-reset.md`, `incident-escalation.md`, `vpn-troubleshooting.md`

> **In plain English:** think of a lockout like a bank card that gets blocked after too many wrong PIN attempts. Unblocking the card is easy. The real work is finding out *what* keeps entering the wrong PIN.

---

## Symptoms

A user may report:

- "Your account has been locked out" message when signing in
- Repeated sign-in failures, even with the correct password
- Outlook or Microsoft 365 apps repeatedly asking for a password
- Unable to connect to VPN or network drives
- Works on one device but not another
- Account locks again shortly after being unlocked

---

## Prerequisites

Before starting, make sure you have:

- **Authorization:** Permission to unlock accounts and view account status
- **Tools (Active Directory environments):**
  - Active Directory Users and Computers (ADUC) or PowerShell with the Active Directory module
  - Access to Security event logs on domain controllers (read-only is enough to investigate)
- **Information from the user:** Username, affected device or service, when the problem started, and whether they recently changed their password
- **Identity verification:** Follow your organization's procedure to confirm who is asking before making any change

> **Note:** Never ask the user for their password, and never record passwords, MFA codes, or tokens in the ticket.

---

## Troubleshooting / Implementation

Follow these steps in order. Do not skip to unlocking: if something keeps causing the lockout, the account will simply lock again.

### 1. Verify the user
1. Confirm the requester's identity using your organization's approved method.
2. Note the username, affected system, symptoms, and when it began.
3. Do not share account details or make security-sensitive changes until verified.

### 2. Confirm the account is actually locked
A locked account is different from a disabled account, an expired account, or an expired password. Each needs a different fix.

**Using ADUC:** open the user's properties and check the **Account** tab.

**Using PowerShell** (example uses a fictional user, `jdoe`):

```powershell
Get-ADUser -Identity jdoe -Properties LockedOut, Enabled, PasswordExpired, BadLogonCount, LastBadPasswordAttempt, AccountExpirationDate |
  Select-Object Name, LockedOut, Enabled, PasswordExpired, BadLogonCount, LastBadPasswordAttempt, AccountExpirationDate
```

| Result | Meaning | Next step |
|---|---|---|
| `LockedOut = True` | Genuine lockout | Continue this runbook |
| `Enabled = False` | Account disabled | Escalate or check with HR/manager. Do not simply re-enable |
| `PasswordExpired = True` | Password needs changing | Use `password-reset.md` |
| Account expiry date passed | Account expired | Escalate to the identity team |
| `LockedOut = False` | Account is not locked | Check username, domain, caps lock, or a different issue |

### 3. Check for a recent password change
A recent password change is the most common cause of repeated lockouts. Ask the user:

- Did you change your password recently?
- Do you have a phone, tablet, or second computer that connects to work email?
- Do you use saved passwords, VPN, or mapped network drives?

### 4. Find the source of the failed attempts
If the account keeps locking, **do not keep unlocking it.** Find what is sending the wrong password.

1. Find the domain controller that handles lockouts (the PDC Emulator):

   ```powershell
   (Get-ADDomain).PDCEmulator
   ```

2. On that domain controller, look for **Event ID 4740** (account locked out). The **Caller Computer Name** field shows which device triggered the lockout:

   ```powershell
   Get-WinEvent -FilterHashtable @{LogName='Security'; Id=4740} -MaxEvents 20 |
     Where-Object { $_.Message -match 'jdoe' } |
     Select-Object TimeCreated, Message
   ```

3. On the computer named in that event, look for **Event ID 4625** (failed logon) to see which process or logon type is responsible.

> **Note:** Available detail depends on your auditing settings. If events are missing, the audit policy may need updating by the identity team.

### 5. Check the user's devices and sessions
Look for any device still using the old password:

- Primary workstation and laptop
- Phone and tablet (email apps are a very common cause)
- Remote Desktop sessions or virtual desktops
- Other computers where the user is still signed in

### 6. Check saved credentials on the affected computer
1. Open **Credential Manager** (search for it in the Start menu).
2. Review **Windows Credentials** and **Generic Credentials** for old entries.
3. Remove or update outdated entries.

Command-line options:

```cmd
cmdkey /list
net use
```

`cmdkey /list` shows saved credentials and `net use` shows current network connections.

### 7. Check mapped drives and network connections
1. Disconnect the affected network drive.
2. Remove any outdated saved credentials for it.
3. Reconnect using the current credentials and confirm access.

### 8. Check scheduled tasks and services
If the account still locks after steps 5 to 7, look for automated items running under the user's account:

- **Task Scheduler:** scripts, backups, and automation tasks
- **Services** (`services.msc`): legacy applications and monitoring tools
- **Other servers:** shared systems where the user's account was used to run something

If a task or service uses a person's account, update its credentials or recommend moving it to a dedicated service account.

### 9. Check for suspicious activity
Escalate immediately (see **When to escalate**) if you see:

- Failed attempts from an unknown device or location
- Many accounts failing at the same time
- A pattern of many failures across many accounts (possible password spraying)
- The user reports sign-in alerts they did not cause

> **Do not unlock the account** if a security investigation is open or you suspect a compromise. Escalate first.

### When to escalate
Use `incident-escalation.md` when:

- The account repeatedly locks with no identifiable cause
- Several users have unexplained lockouts
- Failures come from an unknown device
- You suspect password spraying, brute force, or a compromised account
- The account has elevated (admin) privileges
- A business-critical service is affected
- Fixing it needs access outside the support team's authority

---

## Resolution

### Unlock the account
Once the cause is found or reasonable checks are complete, an authorized technician can unlock the account.

**Using ADUC:** open the user's properties, go to the **Account** tab, tick **Unlock account**, then click **OK**.

**Using PowerShell:**

```powershell
Unlock-ADAccount -Identity jdoe
```

Before unlocking, confirm that:

- The user's identity is verified
- The account should remain active
- The activity does not look suspicious
- No security investigation is in progress

### Fix the underlying cause
Unlocking only clears the symptom. Also:

- Update or remove the old password on the device, app, drive, or task that caused it
- If the password is forgotten or possibly exposed, reset it using `password-reset.md`
- For cloud-only accounts (Microsoft Entra ID), follow your organization's process for smart lockout and self-service password reset

### Document the ticket
Record:

- Date and time, and the username (per your organization's policy)
- Symptoms and when the lockout occurred
- Whether a password change was recent
- What you checked, and the source of the lockout if found
- What you did to resolve it
- Verification results and any escalation

> **Never** record passwords, MFA codes, or tokens in the ticket.

---

## Verification

Confirm the issue is truly fixed, not just temporarily cleared.

- [ ] User identity verified
- [ ] Correct account identified
- [ ] Lockout confirmed (not a disabled or expired account)
- [ ] Recent password change checked
- [ ] Source of failed sign-ins investigated
- [ ] Stored credentials reviewed
- [ ] User devices reviewed
- [ ] Mapped drives checked
- [ ] Scheduled tasks and services considered
- [ ] Account unlocked by an authorized technician
- [ ] User signed in successfully
- [ ] Key apps tested (Windows sign-in, Outlook, Microsoft 365, VPN, network shares)
- [ ] Account still unlocked after some time (check again later the same day)
- [ ] Ticket updated with all troubleshooting performed
- [ ] Security escalation completed if required

To re-check the status quickly:

```powershell
Get-ADUser -Identity jdoe -Properties LockedOut | Select-Object Name, LockedOut
```

> **In plain English:** the user can sign in everywhere they need to, and the account stays unlocked.

---

## Common Lockout Scenarios

| Scenario | Likely cause | What to check |
|---|---|---|
| Locks right after a password change | Old password saved somewhere | Phone, Outlook, VPN, Credential Manager, mapped drives |
| Locks overnight or at the same time daily | Scheduled task or service | Task Scheduler and Services |
| Locks when VPN is used | Saved VPN credentials | VPN client settings |
| Locks repeatedly from one workstation | Local app or cached credentials | That workstation |
| Locks from many devices | Shared or outdated credentials | All user devices and services |
| Locks again right after unlocking | A device is still sending the old password | Event ID 4740 caller computer name |
| Still locked after a password reset | Another system still uses the old password | Source of failed attempts |
| Unknown source of repeated failures | Possible security issue | Investigate and escalate |

---

## Prevention

Organizations can reduce lockouts by:

- **Educating users** about updating their password on all devices after a change
- **Encouraging approved password managers** to avoid typing errors and outdated entries
- **Removing obsolete saved credentials** during device clean-ups
- **Using dedicated service accounts** for scheduled tasks and services, never personal accounts
- **Reviewing scheduled tasks and services** regularly
- **Using MFA** and self-service password reset where appropriate
- **Monitoring authentication failures** to spot patterns early
- **Maintaining accurate account and asset records** so you know where an account is used
- **Setting sensible lockout policies** (threshold, duration, and reset window) that balance security and usability

---

## Lessons Learned

- **Don't just keep unlocking.** If an account keeps locking, something is still sending the wrong password. Find it.
- **Phones are the most common hidden cause.** An old password on a mobile email app can lock an account repeatedly.
- **Check the type of problem first.** A disabled, expired, or password-expired account looks similar to a lockout but needs a different fix.
- **Event ID 4740 saves time.** The caller computer name usually points straight to the source.
- **Personal accounts shouldn't run services or tasks.** Every password change then breaks something.
- **Repeated lockouts can be a security signal.** Treat unexplained patterns as possible attacks until proven otherwise.

### My notes
_Add your own lessons here from lab exercises or real tickets:_

- 

---

## References

- Microsoft Learn: Active Directory Domain Services
- Microsoft Learn: Windows Security Auditing, including Event ID 4740 (account lockout) and Event ID 4625 (failed logon)
- Microsoft Learn: Active Directory PowerShell module (`Get-ADUser`, `Unlock-ADAccount`, `Get-ADDomain`)
- Microsoft Learn: Microsoft Entra ID (smart lockout and self-service password reset)
- Your organization's identity verification, password, and incident response policies
