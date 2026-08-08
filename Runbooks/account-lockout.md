# Account Lockout Troubleshooting Runbook

## Overview

This runbook provides a standardized procedure for troubleshooting and resolving **user account lockouts** in an IT support environment.

An account lockout occurs when a user account is temporarily prevented from authenticating after a defined number of unsuccessful sign-in attempts. Account lockouts are commonly associated with **Active Directory Domain Services (AD DS)** environments, but similar issues can occur with cloud identity platforms and applications that enforce authentication policies.

The objective of this runbook is to identify the cause of the lockout, restore the user's access safely, and determine whether additional action is required.

---

## Purpose

This procedure is intended to help IT Support and Service Desk personnel:

* Identify why a user account has been locked out
* Verify the affected user account
* Determine whether the lockout is legitimate or suspicious
* Unlock an Active Directory user account when appropriate
* Identify devices or applications repeatedly submitting incorrect credentials
* Verify that the user can successfully authenticate
* Escalate suspected security incidents appropriately
* Document the incident and resolution

---

## Scope

This runbook applies to common account lockout scenarios involving:

* Windows domain user accounts
* Active Directory user accounts
* Domain-joined computers
* Microsoft 365 and cloud-connected identities where applicable
* Cached credentials
* Mobile devices
* Email clients
* VPN clients
* Mapped network drives
* Scheduled tasks
* Services configured with user credentials
* Applications using stored credentials

The exact administrative procedures may vary depending on the organization's identity infrastructure and security policies.

---

## Symptoms

A user may report one or more of the following:

* Unable to sign in to Windows
* "Account is locked out" message
* Repeated authentication failures
* Unable to access network resources
* Unable to connect to VPN
* Outlook repeatedly requesting credentials
* Microsoft 365 applications repeatedly requesting authentication
* Access denied after previously working credentials
* User can authenticate on one device but not another
* Account becomes locked again shortly after being unlocked

---

## Common Causes

Account lockouts can occur for several reasons.

### Incorrect Password Attempts

The most straightforward cause is repeated authentication using an incorrect password.

Possible sources include:

* User entering an old password
* Incorrect password being repeatedly entered
* Password changed recently
* Password manager containing an outdated password

### Stored Credentials

Old credentials may remain stored on:

* Windows Credential Manager
* Email applications
* VPN clients
* Network drives
* Mobile devices
* Applications
* Remote desktop sessions

### Mapped Network Drives

A mapped network drive configured with outdated credentials can repeatedly attempt authentication and trigger an account lockout.

### Mobile Devices

A phone or tablet configured with an old password may continue attempting to connect to email or other services.

### Scheduled Tasks

A Windows scheduled task may be configured to run under a user's account with an outdated password.

### Windows Services

A service configured to use a domain user account may continue attempting authentication after the user's password has changed.

### Applications

Applications that store credentials may continue submitting an old password.

### Multiple Devices

The user may be signed in on multiple computers, phones, tablets, or remote sessions, with one device still using outdated credentials.

### Security-Related Activity

Unexpected or repeated authentication failures can potentially indicate:

* Credential theft
* Brute-force attempts
* Password spraying
* Compromised devices
* Unauthorized access attempts

Suspicious activity should be handled according to the organization's security incident response procedures.

---

# Troubleshooting Procedure

## 1. Verify the User

Before making account changes, verify the identity of the person requesting assistance according to organizational identity verification procedures.

Confirm:

* User identity
* Username
* Affected system or service
* Reported symptoms
* Approximate time the issue began

Do not disclose account information or make security-sensitive changes until the requester has been appropriately verified.

---

## 2. Confirm the Account Is Locked

Check the user's account status using the organization's approved identity management tools.

In an Active Directory environment, authorized administrators can check the user account properties in **Active Directory Users and Computers (ADUC)**.

Confirm whether:

* The account is locked
* The account is disabled
* The account has expired
* The password has expired
* The user is entering the correct username
* The user is authenticating against the expected domain

An account that is disabled or expired requires a different resolution from a temporary account lockout.

---

## 3. Determine When the Lockout Occurred

Establish approximately when the account became locked.

Useful information includes:

* Time of the user's last successful login
* Time the lockout was first reported
* Device being used when the problem occurred
* Applications being used
* Recent password changes
* Recent device changes
* Recent VPN or remote-access activity

This information can help narrow down the source of the authentication attempts.

---

## 4. Check for Recent Password Changes

Ask whether the user recently changed their password.

If the password was recently changed, investigate systems that may still contain the previous password.

Common locations include:

* Mobile devices
* Outlook
* VPN clients
* Windows Credential Manager
* Mapped network drives
* Remote Desktop sessions
* Scheduled Tasks
* Windows services
* Third-party applications

A recently changed password is one of the most common causes of repeated account lockouts.

---

## 5. Identify the Source of the Lockout

If the account repeatedly becomes locked after being unlocked, do not simply continue unlocking the account.

Investigate the source of the failed authentication attempts.

In an Active Directory environment, security event logs can provide useful information.

Relevant Windows Security events may include:

* **Event ID 4740** — A user account was locked out
* **Event ID 4625** — An account failed to log on

Event information may help identify:

* The affected account
* The time of the authentication failure
* The originating computer
* The authentication context
* Other information useful for investigation

The exact information available depends on the domain configuration, auditing settings, and authentication method.

---

## 6. Check the User's Devices

Determine whether the user has multiple devices or sessions.

Check for:

* Primary workstation
* Laptop
* Mobile phone
* Tablet
* Remote workstation
* Virtual desktop
* Remote Desktop sessions

A device that has not received the updated password may continue attempting authentication.

---

## 7. Check Stored Credentials

On Windows devices, review **Credential Manager** for outdated credentials where appropriate.

Potential sources include:

* Windows credentials
* Network resource credentials
* Remote access credentials
* Application credentials

Remove or update obsolete credentials according to organizational procedures.

---

## 8. Check Mapped Drives and Network Resources

Review mapped network drives and other persistent network connections.

Look for connections that may still be attempting authentication with an outdated password.

Where necessary:

1. Disconnect the affected network resource.
2. Remove outdated stored credentials.
3. Reconnect using the current credentials.
4. Verify access.

---

## 9. Check Scheduled Tasks and Services

If the account continues to lock after the user's devices and stored credentials have been checked, investigate scheduled tasks and services.

Look for tasks or services configured to run under the affected user account.

Particular attention should be given to:

* Scheduled scripts
* Backup jobs
* Automation tasks
* Legacy applications
* Windows services
* Monitoring systems

If a service or task uses a user account, credentials may need to be updated or the service account design reviewed.

---

## 10. Unlock the Account

Once the cause has been identified or reasonable checks have been completed, an authorized administrator may unlock the account according to organizational procedures.

In an Active Directory environment, this can typically be performed through the user's account properties in Active Directory Users and Computers.

Before unlocking the account:

* Confirm the user's identity
* Confirm the account should remain active
* Consider whether the activity appears suspicious
* Verify that there is no active security investigation

Do not repeatedly unlock an account when an unknown system continues generating authentication failures.

---

## 11. Verify Authentication

After unlocking the account, ask the user to authenticate again.

Verify the required services, such as:

* Windows sign-in
* Microsoft 365 applications
* Outlook
* VPN
* Network shares
* Other business applications

Confirm that the account remains unlocked after authentication.

---

## 12. Monitor for Recurrence

If the account becomes locked again shortly after being unlocked, continue investigating rather than repeatedly unlocking the account.

Possible causes include:

* Another device using an old password
* Cached credentials
* Scheduled task
* Windows service
* Mobile device
* VPN client
* Application
* Remote session
* Suspicious authentication activity

Repeated lockouts without an identified cause should be escalated when appropriate.

---

# Verification Checklist

Use the following checklist to confirm that the issue has been resolved:

* [ ] User identity verified
* [ ] Correct account identified
* [ ] Account lockout confirmed
* [ ] Recent password change checked
* [ ] Potential source of failed authentication investigated
* [ ] Stored credentials reviewed
* [ ] User devices reviewed
* [ ] Mapped drives checked
* [ ] Scheduled tasks/services considered
* [ ] Account unlocked by an authorized administrator
* [ ] User successfully authenticated
* [ ] Required applications tested
* [ ] Account remains unlocked
* [ ] Ticket updated with troubleshooting performed
* [ ] Security escalation completed if required

---

# Escalation Criteria

Escalate the incident to the appropriate administrator, identity team, or security team when:

* The account repeatedly locks without an identifiable cause
* Multiple users experience unexplained account lockouts
* Authentication failures originate from an unknown device
* There are indications of password spraying or brute-force activity
* The user reports suspicious sign-in activity
* The account may have been compromised
* The affected account has elevated privileges
* The issue affects a critical business service
* The issue requires access to systems outside the support team's authorization

Security-related escalation should follow the organization's incident response procedures.

---

# Documentation Requirements

The support ticket should record relevant information such as:

* Date and time of the incident
* Affected username or account identifier according to organizational policy
* Reported symptoms
* Approximate time the lockout occurred
* Recent password changes
* Troubleshooting performed
* Source of the lockout, if identified
* Resolution performed
* Verification results
* Escalation details, if applicable

Never record passwords, MFA codes, authentication tokens, or other secrets in the ticket.

---

# Common Account Lockout Scenarios

| Scenario                                     | Likely Cause                                                 | Recommended Investigation                                |
| -------------------------------------------- | ------------------------------------------------------------ | -------------------------------------------------------- |
| Lockout immediately after password change    | Old credentials stored somewhere                             | Check devices, applications, VPN, and Credential Manager |
| Lockout occurs overnight                     | Scheduled task or service                                    | Review scheduled tasks and services                      |
| Lockout occurs when VPN is used              | Stored VPN credentials                                       | Review VPN configuration and saved credentials           |
| Lockout occurs repeatedly on one workstation | Local application or cached credentials                      | Investigate the workstation                              |
| Lockout occurs across multiple devices       | Shared or outdated credentials                               | Review all user devices and services                     |
| Unknown source causes repeated lockouts      | Potential security issue                                     | Investigate and escalate                                 |
| Account remains locked after password reset  | Another system continues authenticating with old credentials | Identify the source of authentication attempts           |

---

# Prevention

Organizations can reduce recurring account lockouts by:

* Educating users about password changes
* Encouraging password managers where approved
* Removing obsolete stored credentials
* Avoiding the use of personal accounts for services
* Using dedicated service accounts where appropriate
* Reviewing scheduled tasks and services
* Implementing appropriate MFA
* Monitoring authentication failures
* Maintaining accurate asset and account records
* Applying appropriate account lockout and authentication policies

---

# Related Topics

* Active Directory User Management
* Windows Authentication
* Password Reset
* Microsoft Entra ID
* Group Policy
* Windows Security Event Logs
* Identity and Access Management
* Security Incident Response

---

## References

* Microsoft Learn — Active Directory Domain Services
* Microsoft Learn — Windows Security Auditing
* Microsoft Learn — Windows Security Event IDs
* Microsoft Learn — Microsoft Entra ID
