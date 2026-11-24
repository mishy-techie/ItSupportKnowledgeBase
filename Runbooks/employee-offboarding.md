# Employee Offboarding

**Summary:** The process of removing a departing employee's access, recovering company equipment, and preserving business data, so nothing is left open or unaccounted for.

**Environment:** Controlled IT support environment (identity management, HR system, asset inventory)

**Related runbooks:** `incident-escalation.md`, `account-lockout.md`, `new-employee-onboarding.md`

---

## Prerequisites

Before starting, make sure you have:

- **Authorization:** Written approval from HR or the employee's manager, including the last working day and whether the departure is routine or urgent (for example, a dismissal)
- **Access:** Permission to disable accounts, revoke sessions, and manage devices in your identity and endpoint management tools
- **Information:**
  - The employee's role, manager, and department
  - A list of their applications, shared mailboxes, shared drives, and groups
  - A list of equipment assigned to them (laptop, phone, badge, tokens)
  - The company's data-retention policy
- **Contacts:** The manager (for data handover), HR, and the security team (for escalation)

> **In plain English:** get approval, know what the person has access to, and know what equipment they hold before you change anything.

---

## Troubleshooting / Implementation

Follow these steps in order. Record the time and your name against each one.

### 1. Confirm the request
1. Verify the request comes from HR or an authorized manager. Do not act on an unverified message.
2. Confirm the last working day and the exact time access should end.
3. Check whether the departure is **urgent** (access removed immediately) or **planned** (access removed at the end of the last day).

### 2. Preserve data first
1. Ask the manager what data must be kept (email, files, shared documents, project work).
2. Transfer ownership of files and shared resources, or grant the manager access, as policy allows.
3. Set up mailbox handling per policy (for example, forwarding, shared mailbox, or retention hold).
4. Check retention rules **before** deleting or wiping anything.

### 3. Remove access
1. Disable the user account (do not delete it yet, unless policy says otherwise).
2. Reset the password and revoke active sessions and sign-in tokens.
3. Remove or reset MFA methods and saved logins.
4. Remove the user from groups, distribution lists, and shared resources.
5. Remove application and licence access (email, collaboration tools, line-of-business apps).
6. Remove VPN, remote access, and Wi-Fi access.
7. Review and remove any privileged or admin rights.

### 4. Recover equipment
1. Arrange return of all company equipment (laptop, phone, peripherals, badges, security tokens).
2. If equipment is not returned, record its status (lost, pending, not returned) and notify HR and the manager.
3. For company-managed devices, lock or remotely wipe them if policy requires.

### 5. Secure returned devices
1. Check the device in and update the asset record.
2. Back up any required data, then wipe or reimage according to policy.
3. Store or reissue the device and record its new status.

### 6. Close out
1. Update asset and access records.
2. Confirm any scheduled actions (such as account deletion after a retention period) are set.
3. Work through the **Verification** checklist below.
4. Escalate anything unusual (see **When to escalate**).

### When to escalate
Use `incident-escalation.md` if you see any of the following:

- Signs of data theft, unusual downloads, or unexpected forwarding rules
- Sign-ins after the account was disabled
- Unreturned equipment that holds sensitive data
- An urgent or hostile departure where immediate access removal is needed
- Admin rights or systems you cannot remove yourself

### Common problems

| Problem | Likely cause | What to do |
|---|---|---|
| User can still sign in after the account is disabled | Active session or token not revoked | Revoke all sessions and refresh tokens, then retest |
| User still receives email on a phone | Mobile device still registered | Remove the device or wipe the work profile |
| Manager cannot open the former employee's files | Ownership not transferred | Transfer ownership or grant access as policy allows |
| Laptop was not returned | Employee has left or equipment is lost | Record status, notify HR, lock or wipe remotely if possible |
| An application still lists the user as active | Separate login not tied to central identity | Remove the user directly in that application |

---

## Resolution

The request is resolved when:

1. Every step in **Troubleshooting / Implementation** is done, or any exception is documented with a reason.
2. The **Verification** checklist is fully ticked.
3. HR and the manager are told the offboarding is complete.
4. The ticket is closed with a summary of what was done, what was transferred, and any equipment still outstanding.

Do not close the ticket while critical actions remain open. Escalate or assign them to an owner instead.

---

## Verification

An offboarding request is complete only when every item below is confirmed. Tick each box and record who confirmed it and when.

### Access removed
- [ ] Access termination was authorized (HR or manager approval on record)
- [ ] User account is disabled, or handled according to company policy
- [ ] Active sessions and sign-in methods (passwords, MFA, saved logins) are revoked
- [ ] Application access is removed
- [ ] Privileged or admin access has been reviewed and removed

### Data and equipment
- [ ] Business-critical data is preserved or transferred (for example, to the manager)
- [ ] Company equipment is returned, or its status is documented (lost, pending, not returned)
- [ ] Returned devices are secured (wiped, locked, or stored per policy)
- [ ] Asset records are updated
- [ ] Data-retention requirements are addressed

### Wrap-up
- [ ] Any security concerns are escalated (see `incident-escalation.md`)
- [ ] Documentation is complete
- [ ] No critical offboarding actions remain unresolved

> **In plain English:** the person can no longer get into anything, the company's data and equipment are accounted for, and there is a record proving it.

---

## Prevention

Most offboarding problems come from manual steps, missed systems, or poor communication. These practices reduce the risk of something being forgotten.

### Automate where possible
- **Automate account deprovisioning:** Accounts are disabled automatically, not by memory
- **Automate session revocation:** Signed-in devices and apps are logged out immediately
- **Integrate HR and identity systems:** When HR records a departure, IT is notified automatically

### Control who has access
- **Use role-based access controls:** Access is tied to job roles, so removing the role removes the access
- **Use centralized identity management:** One place to disable a user, instead of many separate systems
- **Review privileged access regularly:** Admin rights are checked on a schedule, not just at departure
- **Conduct periodic access reviews:** Catch accounts and permissions that should no longer exist

### Keep clear records
- **Maintain standardized offboarding checklists:** The same steps are followed every time
- **Maintain accurate asset inventories:** You know what equipment each person holds
- **Track equipment returns:** Unreturned devices are chased, not forgotten
- **Maintain documented data-retention procedures:** Clear rules on what is kept, for how long, and who owns it

### Learn from mistakes
- **Audit completed offboarding requests:** Spot-check that nothing was missed
- **Review recurring failures:** If the same step keeps being missed, fix the process, not just the ticket

---

## Lessons Learned

Key takeaways from this process. Add real examples from your own lab or work as you gain experience.

- **Preserve data before removing access.** Once an account is deleted, recovering its data can be difficult or impossible.
- **Disable first, delete later.** Disabling is reversible; deletion often is not.
- **Disabling the account is not enough.** Active sessions, tokens, mobile devices, and separate app logins can keep working until revoked.
- **Hidden access is the biggest risk.** Shared mailboxes, group memberships, and apps with their own logins are the most commonly missed items.
- **Equipment tracking needs good records.** Without an accurate asset inventory, you cannot tell what is missing.
- **Communication matters as much as technology.** Late or incomplete notice from HR is the most common cause of delayed offboarding.

### My notes
_Add your own lessons here from lab exercises or real offboarding experience:_

- 

---

## References

Use official documentation for the platforms in your environment.

- NIST SP 800-53, Security and Privacy Controls (see the Account Management control family, AC-2)
- CIS Critical Security Controls (Account Management and Access Control Management)
- Microsoft Learn: Microsoft Entra ID and Microsoft 365 documentation on disabling users, revoking sessions, and mailbox handling
- Microsoft Learn: Active Directory documentation on disabling and managing user accounts (if applicable)
- Your organization's data-retention, acceptable-use, and asset-management policies
