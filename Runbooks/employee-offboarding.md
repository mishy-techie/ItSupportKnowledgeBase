# Employee Offboarding Runbook

## Overview

This runbook provides a standardized procedure for **employee offboarding** from an IT support and access management perspective.

Effective IT offboarding ensures that organizational access is removed promptly, company-owned equipment is recovered, business data is protected, and asset records are updated when an employee leaves the organization or changes roles.

The objective of this runbook is to provide IT Support and Service Desk personnel with a repeatable process for securely disabling access, recovering equipment, transferring required business information, and documenting completion.

---

## Purpose

This procedure is intended to help IT Support personnel:

* Disable employee accounts
* Remove access to organizational systems
* Revoke active sessions and authentication methods
* Recover company-owned equipment
* Protect organizational data
* Transfer business information appropriately
* Remove application access
* Update asset records
* Maintain an auditable offboarding process
* Reduce the risk of unauthorized access after an employee leaves

---

## Scope

This runbook applies to the IT aspects of offboarding for:

* Employees
* Contractors
* Interns
* Temporary staff
* Remote workers
* Employees transferring to another department where access changes are required

The exact procedure depends on the organization's identity provider, HR system, device management platform, applications, security policies, and data retention requirements.

---

# Offboarding Request

## 1. Confirm the Offboarding Request

Verify that the offboarding request has been submitted through the organization's approved process.

Confirm:

* [ ] Employee name
* [ ] Username
* [ ] Department
* [ ] Manager
* [ ] Last working date
* [ ] Effective access termination date and time
* [ ] Employment status
* [ ] Reason or category where required by organizational policy
* [ ] Whether the employee is remote or office-based
* [ ] Required equipment to be recovered
* [ ] Required data transfer
* [ ] Special access considerations

Do not disable accounts based solely on an informal request without the required authorization.

---

# Offboarding Priority

## 2. Determine the Required Timing

Confirm whether the offboarding is:

* Scheduled
* Immediate
* Security-related
* Related to a role transfer
* Related to contract completion

For immediate or security-sensitive offboarding, coordinate with the appropriate HR, management, identity, and security teams.

Access termination should follow the organization's approved offboarding schedule and authorization process.

---

# Account Management

## 3. Disable the User Account

At the authorized time:

1. Disable the user's organizational account.
2. Verify the account is no longer active.
3. Confirm that authentication is blocked.
4. Document the action.

Do not delete the account immediately unless organizational policy specifically requires it.

Retaining the account in a disabled state may be necessary for audit, investigation, data retention, or administrative purposes.

---

## 4. Revoke Active Sessions

Where supported by the organization's identity platform, revoke active sessions and authentication tokens.

Check for:

* Active browser sessions
* Cloud application sessions
* Mobile sessions
* VPN sessions
* Remote access sessions
* Authentication tokens

This helps prevent continued access from previously authenticated devices.

---

## 5. Disable MFA and Authentication Methods

Follow the organization's identity management procedure to remove or disable authentication methods associated with the departing employee.

Review:

* MFA devices
* Authentication applications
* Security keys
* Registered phone numbers
* Recovery methods
* Trusted devices

Do not remove authentication information before confirming the required audit and security procedures.

---

# Application Access

## 6. Remove Application Access

Review applications assigned to the employee.

Remove or disable access to required systems, including:

* Business applications
* Cloud services
* Collaboration platforms
* File storage
* CRM systems
* HR systems
* Financial applications
* Development platforms
* Administrative tools
* Remote access systems

Where application access is controlled through centralized identity groups, removing the relevant account or group membership may automatically revoke access.

---

## 7. Review Privileged Access

Determine whether the employee had elevated permissions.

Check for:

* Administrator accounts
* Privileged groups
* Server access
* Cloud administrator roles
* Database access
* Security administration
* Network administration
* Application administration
* Developer credentials

Privileged access should be reviewed carefully and escalated to the appropriate security or infrastructure team where required.

---

# Email and Collaboration

## 8. Secure the Email Account

Follow organizational policy for handling the employee's mailbox.

Possible actions include:

* Disable sign-in
* Preserve the mailbox
* Configure an approved automatic response
* Transfer ownership where authorized
* Grant approved delegate access
* Retain data according to policy

Do not automatically forward all incoming email to another employee unless explicitly authorized.

---

## 9. Handle Collaboration Accounts

Review collaboration services such as:

* Microsoft Teams
* Slack
* Zoom
* Other approved communication platforms

Determine whether:

* Account access should be disabled
* Ownership needs to be transferred
* Business-critical files need to be preserved
* Meetings or resources need reassignment
* Team memberships need updating

Follow organizational retention and data-handling policies.

---

# File and Data Management

## 10. Identify Business-Critical Data

Determine whether the employee has business data stored in:

* OneDrive or equivalent cloud storage
* SharePoint
* Network drives
* Local computer storage
* Shared folders
* Business applications
* Project repositories
* Email

Work with the employee's manager or designated data owner to determine what information needs to be retained or transferred.

---

## 11. Transfer Required Business Data

Where authorized:

* Identify required files
* Identify the approved recipient or data owner
* Transfer ownership where supported
* Verify the transfer
* Preserve required permissions
* Document completion

Do not transfer personal or unnecessary data simply because it exists on the employee's device or account.

---

# Device Recovery

## 12. Identify Assigned Equipment

Review the asset management system and identify equipment assigned to the employee.

Examples include:

* Laptop
* Desktop computer
* Mobile phone
* Tablet
* Monitor
* Docking station
* Keyboard
* Mouse
* Headset
* Security keys
* Power adapters
* Other company-owned equipment

Confirm the expected equipment list before recovery.

---

## 13. Recover Company Equipment

For office-based employees:

* [ ] Collect laptop
* [ ] Collect power adapter
* [ ] Collect docking station
* [ ] Collect monitors
* [ ] Collect peripherals
* [ ] Collect mobile devices
* [ ] Collect security keys
* [ ] Confirm equipment condition

For remote employees, follow the organization's approved equipment return and shipping process.

---

## 14. Inspect Returned Equipment

Check:

* [ ] Device is physically present
* [ ] Asset tag matches records
* [ ] Serial number matches records
* [ ] Accessories are accounted for
* [ ] Physical condition documented
* [ ] Damage documented where applicable

Do not access an employee's personal files or personal accounts during equipment inspection.

---

# Device Security

## 15. Secure the Returned Device

Once the device is recovered:

1. Confirm the device is no longer assigned for active use.
2. Place it into the appropriate asset status.
3. Preserve required business data according to policy.
4. Follow approved device wipe or reimaging procedures.
5. Remove previous user configuration where required.
6. Prepare the device for storage, reassignment, or disposal.

Do not wipe a device before confirming that required business data has been preserved.

---

## 16. Revoke Device Access

Where applicable, remove the device from active user assignments.

Check:

* Device management platform
* Endpoint security platform
* Identity platform
* VPN systems
* Mobile device management
* Certificate infrastructure

The device may need to remain enrolled for security or investigation purposes depending on the circumstances of the departure.

---

# Mobile Devices

## 17. Process Company Mobile Devices

For company-issued phones or tablets:

* [ ] Recover device
* [ ] Verify asset information
* [ ] Confirm device ownership
* [ ] Remove user assignment
* [ ] Review mobile device management status
* [ ] Protect business data
* [ ] Perform approved device reset where required
* [ ] Update asset records

Do not remove a device from management before confirming the appropriate security and data-retention requirements.

---

# Access Review

## 18. Perform Final Access Review

Confirm that access has been removed from required systems.

Review:

* [ ] Organizational account
* [ ] Email
* [ ] MFA
* [ ] VPN
* [ ] Cloud applications
* [ ] Business applications
* [ ] File storage
* [ ] Network drives
* [ ] Remote desktop
* [ ] Privileged accounts
* [ ] Administrative groups
* [ ] Collaboration platforms
* [ ] Mobile device management
* [ ] Other role-specific systems

The exact systems reviewed should reflect the employee's role.

---

# Asset Management

## 19. Update Asset Records

Update the organization's asset management system.

Record:

* Asset status
* Returned date
* Device condition
* Assigned user
* New asset status
* Accessories returned
* Damage where applicable
* Storage or reassignment status

Ensure the physical equipment matches the asset records.

---

# Account Retention and Deletion

## 20. Follow Data Retention Requirements

Do not immediately delete user accounts, mailboxes, files, or devices unless required by organizational policy.

Determine:

* Required retention period
* Legal or regulatory requirements
* Business retention requirements
* Security investigation requirements
* Manager or data owner requirements

Coordinate with the appropriate legal, HR, compliance, or security teams when necessary.

---

# Security Considerations

## 21. Review for Security Risks

For standard offboarding, complete the approved access removal process.

For involuntary, immediate, or security-sensitive departures, coordinate with the appropriate security and management teams.

Potential indicators requiring additional attention include:

* Privileged access
* Suspicious recent activity
* Unusual data transfers
* Unauthorized devices
* Unknown authentication sessions
* Unexpected account activity
* Missing company equipment

Do not conduct an independent investigation beyond your authorization. Escalate suspected security incidents through the organization's incident response process.

---

# Common Offboarding Scenarios

| Scenario                               | Required Action                      | Additional Consideration                          |
| -------------------------------------- | ------------------------------------ | ------------------------------------------------- |
| Standard employee departure            | Disable account and remove access    | Recover equipment and preserve required data      |
| Immediate termination                  | Coordinate immediate access removal  | Follow security and HR procedures                 |
| Remote employee departure              | Disable access and recover equipment | Follow approved return/shipping process           |
| Contractor completion                  | Disable account and remove access    | Verify all temporary access is revoked            |
| Department transfer                    | Review and modify access             | Do not necessarily disable the employee's account |
| Lost company laptop during offboarding | Disable access and secure device     | Escalate as a potential security incident         |
| Employee had privileged access         | Revoke elevated permissions          | Conduct additional access review                  |
| Business-critical mailbox              | Preserve and transfer appropriately  | Follow approved data-retention procedure          |
| Missing company equipment              | Document missing assets              | Escalate according to asset and security policy   |

---

# Verification Checklist

* [ ] Offboarding request authorized
* [ ] Last working date and access termination time confirmed
* [ ] User account disabled
* [ ] Active sessions revoked
* [ ] Authentication methods reviewed
* [ ] MFA access handled
* [ ] Application access removed
* [ ] Privileged access reviewed
* [ ] Email access secured
* [ ] Collaboration access reviewed
* [ ] Business data identified
* [ ] Required data transferred or preserved
* [ ] Company equipment identified
* [ ] Equipment recovered
* [ ] Equipment condition documented
* [ ] Device access reviewed
* [ ] Returned devices secured
* [ ] Mobile devices processed where applicable
* [ ] Asset records updated
* [ ] Retention requirements reviewed
* [ ] Final access review completed
* [ ] Security concerns escalated where required
* [ ] Offboarding ticket updated
* [ ] Completion confirmed

---

# Escalation Criteria

Escalate the offboarding request to the appropriate HR, security, identity, infrastructure, legal, compliance, or management team when:

* Access termination timing is unclear
* Authorization is missing
* The employee has privileged access
* The employee is involved in a security investigation
* Company equipment is missing
* Business-critical data requires special handling
* Legal or regulatory retention requirements apply
* An account cannot be disabled
* Active sessions cannot be revoked
* Application access cannot be removed
* Data ownership cannot be transferred
* A device cannot be securely wiped
* Suspicious activity is identified
* The employee has access to critical infrastructure

Provide the escalation team with:

* Employee identifier
* Offboarding effective date and time
* Systems affected
* Access removed
* Access remaining
* Equipment status
* Data handling status
* Error messages
* Troubleshooting performed
* Security concerns
* Outstanding actions

---

# Documentation Requirements

Record the following information in the offboarding ticket:

* Employee identifier
* Department
* Manager
* Offboarding date
* Access termination time
* Account status
* Systems reviewed
* Access removed
* Equipment recovered
* Asset information
* Data transfer or retention actions
* Security actions
* Outstanding issues
* Escalations
* Completion status

Do not record passwords, MFA codes, authentication tokens, recovery keys, or other sensitive credentials.

---

# Prevention and Process Improvement

Organizations can improve employee offboarding by:

* Automating account deprovisioning
* Integrating HR and identity systems
* Using role-based access controls
* Maintaining accurate asset inventories
* Maintaining standardized offboarding checklists
* Reviewing privileged access regularly
* Automating session revocation
* Maintaining documented data-retention procedures
* Tracking equipment returns
* Using centralized identity management
* Conducting periodic access reviews
* Auditing completed offboarding requests
* Reviewing recurring offboarding failures

---

# Completion Criteria

The IT offboarding request can be considered complete when:

* Authorized access termination has occurred
* User account is disabled or otherwise handled according to policy
* Active sessions and authentication methods are addressed
* Application access is removed
* Privileged access is reviewed
* Business-critical data is preserved or transferred appropriately
* Company equipment is recovered or its status documented
* Returned devices are secured
* Asset records are updated
* Data-retention requirements are addressed
* Security concerns are escalated where necessary
* Documentation is complete
* No unresolved critical offboarding actions remain
