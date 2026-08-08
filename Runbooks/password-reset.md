# Password Reset

## Overview

This runbook provides a standardized procedure for handling user password reset requests.

Password reset requests are common IT Support incidents and may occur when a user:

* Forgets their password
* Cannot authenticate to their account
* Has been required to change an expired password
* Is unable to access a service after a password change
* Has been locked out due to repeated failed authentication attempts

The objective is to restore the user's access securely while ensuring that the identity of the requester is appropriately verified.

---

## Purpose

The purpose of this runbook is to provide IT Support personnel with a consistent process for:

1. Verifying the user's identity
2. Determining the reason for the password reset
3. Performing the password reset through the organization's approved identity-management system
4. Confirming that access has been restored
5. Documenting the support request
6. Identifying situations that require escalation

---

## Scope

This runbook applies to standard user password reset requests handled by the IT Support or Service Desk team.

It is intended for use with organizational user accounts and should be adapted to the organization's identity platform and security policies.

Examples of identity platforms may include:

* Active Directory
* Microsoft Entra ID
* Hybrid Active Directory environments
* Other approved identity-management platforms

---

## Security Considerations

Password resets involve access to user accounts and therefore represent a security-sensitive activity.

Before resetting a password:

* Verify the user's identity using the organization's approved verification procedure.
* Never ask the user to provide their existing password.
* Never record passwords in tickets or documentation.
* Never send passwords through insecure communication channels.
* Follow the organization's password policy.
* Use only approved administrative tools.
* Do not bypass identity verification because a request appears urgent.
* Treat unusual or suspicious reset requests as potential security incidents.

If the request appears suspicious, stop the reset process and follow the organization's security escalation procedure.

---

## Symptoms / Trigger

This runbook may be initiated when a user reports:

* "I forgot my password."
* "My password isn't working."
* "I can't log in."
* "I need my password reset."
* "My password has expired."
* "I changed my password but cannot access my applications."

Before performing a reset, determine whether the problem is actually a password issue.

---

## Prerequisites

The support technician should have:

* Access to the organization's approved identity-management system
* Appropriate administrative permissions
* Access to the organization's service desk/ticketing system
* Knowledge of the organization's identity-verification procedure
* Knowledge of the organization's password policy
* Access to escalation procedures

---

## Initial Checks

Before resetting the password, gather the following information:

### 1. Identify the User

Confirm:

* User's full name
* Username or account identifier
* Department or team
* Contact information, where appropriate

### 2. Understand the Problem

Determine:

* What application or service is the user attempting to access?
* Is the issue occurring on one device or multiple devices?
* Has the user recently changed their password?
* Is the account reporting that the password is incorrect?
* Has the user received an account-lockout message?
* Is the password expired?
* Are other users experiencing the same issue?

### 3. Check for Broader Issues

Before resetting the password, determine whether there is a wider authentication problem.

For example:

```text
One user affected
        ↓
Likely account-specific issue
        ↓
Multiple users affected
        ↓
Investigate authentication/service availability
```

If multiple users are experiencing authentication failures, avoid performing unnecessary individual password resets and investigate whether there is a broader service issue.

---

## Identity Verification

Identity verification must be completed **before** resetting the password.

Follow the organization's approved identity-verification process.

Depending on organizational policy, this may involve approved methods such as:

* Verification through a known communication channel
* Verification using approved identity information
* Manager or authorized contact verification
* Approved multi-factor authentication verification
* Service Desk identity-verification procedures

Do not create your own verification process or weaken existing security controls.

### Important

If the user's identity cannot be adequately verified:

**Do not perform the password reset.**

Escalate the request according to organizational policy.

---

## Password Reset Procedure

Once the user's identity has been verified:

### Step 1 — Locate the User Account

Open the organization's approved identity-management platform.

Search for the user's account using the appropriate identifier.

Confirm that the correct account has been selected.

Verify relevant information such as:

* Username
* Display name
* Department
* Account status

Avoid making changes to an account until the correct user has been confirmed.

---

### Step 2 — Check Account Status

Before resetting the password, check whether the account has additional problems.

Review whether the account is:

* Active
* Disabled
* Locked
* Expired
* Subject to another authentication restriction

If the account is disabled, locked, or otherwise restricted, follow the appropriate account-management procedure rather than treating the issue as a simple password reset.

---

### Step 3 — Reset the Password

Use the organization's approved password-reset function.

Apply the organization's password requirements, including applicable requirements for:

* Password length
* Password complexity
* Password history
* Password expiration
* Temporary passwords
* Password-change requirements

Never document the actual password in the support ticket.

---

### Step 4 — Communicate the Reset

Inform the user through an approved communication method that the reset has been completed.

If a temporary password is issued, follow the organization's approved procedure for securely delivering it.

The user should be instructed to change a temporary password when required by organizational policy.

---

### Step 5 — Verify Account Access

Ask the user to attempt authentication again.

Where appropriate, verify access to the affected service.

Check for:

* Successful authentication
* Successful access to required applications
* MFA prompts functioning correctly
* No account-lockout messages
* No additional authentication errors

---

## Common Post-Reset Issues

A successful password reset does not always immediately resolve every authentication problem.

If the user still cannot sign in, investigate:

### Cached Credentials

The user's device or application may still be using previously cached credentials.

### Multiple Devices

The user may be signed in on another device using the old password.

Examples include:

* Laptop
* Mobile phone
* Tablet
* Remote workstation

### Email and Mobile Applications

Applications such as email clients may continue attempting authentication with outdated credentials.

### VPN

A VPN client may have stored or cached authentication information.

### Account Lockout

Another device or service may continue attempting authentication with the previous password and cause the account to become locked again.

### Synchronization

In hybrid identity environments, password changes may require synchronization between identity systems.

If synchronization appears to be delayed or failing, escalate according to the appropriate identity-management procedure.

---

## Verification

The incident can be considered resolved when:

* The user's identity was successfully verified
* The password reset was completed successfully
* The user can authenticate successfully
* The affected application or service is accessible
* No unexpected account restrictions remain
* The user understands any required next steps

---

## Documentation Requirements

Record the following information in the support ticket:

* Date and time of the request
* User/account affected
* Reported issue
* Identity-verification method or confirmation, according to policy
* Troubleshooting performed
* Action taken
* Result
* Any additional issues identified
* Escalation details, if applicable

### Never document:

* The user's password
* Temporary passwords
* MFA codes
* Authentication tokens
* Security answers
* Other authentication secrets

---

## Escalation

Escalate the request when:

* The user's identity cannot be verified
* The account is disabled
* The account repeatedly becomes locked
* Multiple users are experiencing authentication failures
* Password synchronization appears to be failing
* The identity-management platform is unavailable
* There is evidence of unauthorized account activity
* The request appears suspicious
* The technician does not have sufficient permissions
* The issue requires changes outside the Service Desk's authorization

For suspected account compromise or other security-related events, follow the organization's security incident-response procedure.

---

## Prevention / Recommendations

Where appropriate, provide the user with guidance on:

* Using a password manager approved by the organization
* Keeping recovery information up to date
* Enabling and maintaining MFA
* Avoiding password reuse
* Never sharing passwords
* Reporting suspicious authentication activity
* Contacting IT Support rather than attempting to bypass security controls

---

## Troubleshooting Decision Flow

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
    Verified   Not Verified
       │         │
       ▼         ▼
Check account   Do not reset
    status      password
       │         │
       ▼         ▼
Is account     Escalate
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
       User tests login
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

## Related Runbooks

* [Account Lockout](account-lockout.md)
* [Incident Escalation](incident-escalation.md)

---

## References

When implementing this procedure in a real environment, consult the organization's:

* Password policy
* Identity and access management policy
* Account recovery policy
* Identity-verification procedure
* Security incident-response procedure
* Service Desk procedures

---

## Status

**Status:** Draft

**Last Updated:** 2026-08-08
