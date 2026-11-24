# Password Reset

**Summary:** How to safely reset a user's password. The key rule is that identity is verified **before** anything is changed, because a password reset is one of the most common ways attackers take over accounts.

**Environment:** Controlled IT support environment (Active Directory, Microsoft Entra ID, hybrid identity, or another approved identity-management platform; ticketing system)

**Status:** Draft | **Last updated:** 2026-11-24

**Related runbooks:** `account-lockout.md`, `new-employee-onboarding.md`, `incident-escalation.md`

> **In plain English:** a password reset is like replacing a lost key. Before handing over a new one, you must be sure the person asking really owns the door.

---

## Security Considerations

A password reset gives access to an account, so treat it as a security-sensitive action.

- Verify identity using the organization's approved process **before** resetting.
- Never ask for the user's existing password.
- Never record passwords in tickets or documentation.
- Never send passwords through insecure channels (plain email, public chat, text in the clear).
- Follow the password policy and use only approved administrative tools.
- Do not skip verification because a request seems urgent. Urgency is a common attacker tactic.
- Treat unusual or suspicious requests as possible security incidents.

If a request looks suspicious, **stop the reset** and follow your security escalation procedure.

**Warning signs of a social-engineering attempt:**

- Pressure to act immediately ("I'm about to present to the CEO")
- Refusal or inability to complete verification
- A request to send the new password to a different email or phone number
- A request involving a privileged or executive account
- A caller who knows a lot about the person but cannot pass verification

---

## Symptoms

Start this runbook when a user says:

- "I forgot my password."
- "My password isn't working."
- "I can't log in."
- "My password has expired."
- "I changed my password but can't get into my applications."

Before resetting, check the problem is actually a **password** problem. It may be a lockout, a disabled account, or a wider outage.

---

## Prerequisites

Before starting, make sure you have:

- **Access:** the approved identity-management system and the permission to reset passwords
- **Ticketing:** access to the service desk or ticketing system
- **Procedures:** the organization's identity-verification procedure and password policy
- **Escalation path:** contacts for the identity and security teams

---

## Troubleshooting / Implementation

### Step 1: Identify the user and the problem
Confirm:

- Full name, username or account ID, and department
- What the user is trying to access
- Whether it happens on one device or several
- Whether they recently changed their password
- Whether they see "incorrect password", "account locked", or "password expired"
- Whether other users are affected

### Step 2: Check for a wider problem
```text
One user affected
        ↓
Likely an account-specific issue
        ↓
Multiple users affected
        ↓
Investigate authentication or service availability
```

If several users cannot sign in, do not reset passwords one by one. Investigate a wider authentication outage and escalate (see `incident-escalation.md`).

### Step 3: Verify the user's identity
Complete verification **before** any reset, using your organization's approved method. Depending on policy this might be:

- Verification through a known, trusted contact channel
- Approved identity information
- Manager or authorized-contact confirmation
- Approved MFA verification
- Your Service Desk verification procedure

Do not invent your own process or weaken existing controls.

> **If identity cannot be verified:** do not reset the password. Escalate according to policy.

### Step 4: Locate and check the account
1. Open the approved identity platform and find the account.
2. Confirm it is the correct user (username, display name, department).
3. Check the status: **active, disabled, locked, expired, or otherwise restricted**.

| Account status | What to do |
|---|---|
| Active and not locked | Continue with the reset |
| Locked out | Follow `account-lockout.md` (a reset alone may not fix the cause) |
| Disabled | Do not re-enable it as part of a reset. Escalate or check with HR or the manager |
| Expired account | Escalate to the identity team |
| Password expired | Continue with the reset |
| Other restriction | Follow the relevant account-management procedure |

Example check (Active Directory, fictional user `jdoe`):

```powershell
Get-ADUser -Identity jdoe -Properties Enabled, LockedOut, PasswordExpired, PasswordLastSet, AccountExpirationDate |
  Select-Object Name, Enabled, LockedOut, PasswordExpired, PasswordLastSet, AccountExpirationDate
```

### Step 5: Reset the password
Use the approved reset function and apply the password policy (length, complexity, history, expiration, and temporary password rules).

Example (Active Directory). The password is typed at a hidden prompt so it never appears on screen or in a script:

```powershell
Set-ADAccountPassword -Identity jdoe -Reset -NewPassword (Read-Host -AsSecureString "New temporary password")
Set-ADUser -Identity jdoe -ChangePasswordAtLogon $true
```

In Microsoft Entra ID or Microsoft 365, use the admin center's **Reset password** option and choose to require a password change at next sign-in, where your policy requires it.

> **Preferred option:** if your organization uses **self-service password reset (SSPR)**, direct the user to it first. It is quicker, and it reduces the risk of tricking the Service Desk.

### Step 6: Communicate the reset securely
- Tell the user through an **approved** channel that the reset is complete.
- Deliver any temporary password using the approved secure method, separate from the username where policy requires.
- Remind the user to change a temporary password when prompted.

### Step 7: Confirm access
Ask the user to sign in again, and where appropriate confirm:

- [ ] Sign-in succeeds
- [ ] Required applications are reachable
- [ ] MFA prompts work correctly
- [ ] No lockout message appears
- [ ] No other authentication errors appear

### Common post-reset issues
A reset does not always fix everything immediately. If the user still cannot sign in, check:

| Issue | Why it happens | What to do |
|---|---|---|
| **Cached credentials** | Device or app still has the old password | Sign out and in again; clear saved credentials |
| **Other devices** | Phone, tablet, or second computer still uses the old password | Update the password on every device |
| **Email and mobile apps** | Apps keep retrying the old password | Update the password in each app |
| **VPN client** | Saved old credentials | Update or clear the saved VPN credentials |
| **Account lockout** | Another device keeps sending the old password and locks the account again | Follow `account-lockout.md` |
| **Synchronization delay** | In hybrid setups, the change must sync between systems | Wait a few minutes; if it persists, escalate to the identity team |

For hybrid environments, a sync delay or failure is normally handled by the identity team.

### When to escalate
Use `incident-escalation.md` when:

- The user's identity cannot be verified
- The account is disabled
- The account keeps locking
- Several users have authentication failures
- Password synchronization appears to be failing
- The identity platform is unavailable
- There is evidence of unauthorized account activity, or the request looks suspicious
- You do not have enough permissions
- The change is outside the Service Desk's authority

For suspected account compromise, follow your security incident response procedure.

### Decision flow

```text
User reports password problem
            │
            ▼
     Identify the user
            │
            ▼
    Verify user's identity
            │
       ┌────┴────┐
       │         │
    Verified   Not verified
       │         │
       ▼         ▼
Check account   Do not reset
    status      the password
       │         │
       ▼         ▼
Is account      Escalate
restricted?
       │
   ┌───┴────┐
   │        │
  Yes       No
   │        │
   ▼        ▼
Follow      Reset
account     password
procedure      │
              ▼
       User tests sign-in
              │
        ┌─────┴─────┐
        │           │
     Success      Failure
        │           │
        ▼           ▼
   Document      Investigate
   resolution    further
                     │
                     ▼
                  Escalate
                  if required
```

---

## Resolution

The request is resolved when the password has been reset, the user can sign in, and the ticket is documented.

### Document the ticket
Record:

- Date and time of the request
- User and account affected
- Reported issue
- Identity-verification method or confirmation (according to policy)
- Troubleshooting performed and action taken
- Result and any extra issues found
- Escalation details, if any

### Never document
- The user's password or any temporary password
- MFA codes or authentication tokens
- Security answers
- Any other authentication secrets

---

## Verification

- [ ] User's identity verified before the reset
- [ ] Correct account confirmed
- [ ] Account status checked (not disabled, locked, or expired)
- [ ] Wider outage ruled out
- [ ] Password reset completed through the approved tool
- [ ] Password delivered securely, and a change required if the policy says so
- [ ] User signed in successfully
- [ ] Affected application or service accessible
- [ ] MFA works
- [ ] No lockout or other restrictions remain
- [ ] User understands any next steps
- [ ] Ticket documented with no secrets recorded

Quick check (Active Directory):

```powershell
Get-ADUser -Identity jdoe -Properties LockedOut, PasswordLastSet | Select-Object Name, LockedOut, PasswordLastSet
```

> **In plain English:** the right person got in, the account is healthy, and nothing sensitive was written down.

---

## Prevention

Where appropriate, give users this guidance, and consider organizational controls too.

### For users
- Use an approved password manager
- Keep recovery information (phone, backup email) up to date
- Turn on and maintain MFA
- Never reuse passwords or share them
- Report suspicious sign-in activity or password-reset emails they did not request
- Contact IT Support rather than trying to get around security controls

### For the organization
- **Enable self-service password reset** with strong verification, so fewer resets go through the Service Desk
- **Use MFA** so a stolen password alone is not enough
- **Use a clear, enforced identity-verification procedure** for all reset requests
- **Apply modern password policy** (length over frequent forced changes, and blocking known-weak passwords)
- **Monitor resets** for unusual patterns, such as many requests in a short time or resets on privileged accounts
- **Train Service Desk staff** to recognize social-engineering attempts

---

## Lessons Learned

- **Verify first, always.** A fast reset for the wrong person is far worse than a slow reset for the right one.
- **Check the account status before resetting.** A lockout, disabled account, or outage will not be fixed by a new password.
- **A reset can cause a lockout.** Old passwords saved on phones and apps keep trying and lock the account again.
- **Urgency is a red flag.** Pressure to skip steps should make you more careful, not less.
- **Keep secrets out of tickets.** Temporary passwords and MFA codes should never appear in notes.
- **Self-service is safer and faster** when it is properly set up.
- **Several users failing at once is an incident**, not a pile of password resets.

### My notes
_Add your own lessons here from lab exercises or real tickets:_

- 

---

## References

- Microsoft Learn: Microsoft Entra self-service password reset (SSPR) and password policies
- Microsoft Learn: Active Directory PowerShell module (`Get-ADUser`, `Set-ADAccountPassword`, `Set-ADUser`)
- NIST SP 800-63B, Digital Identity Guidelines: Authentication and Lifecycle Management
- Your organization's password policy, identity and access management policy, and account recovery policy
- Your organization's identity-verification, Service Desk, and security incident response procedures
