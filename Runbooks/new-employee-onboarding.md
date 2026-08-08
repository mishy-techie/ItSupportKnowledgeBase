# New Employee Onboarding Runbook

## Overview

This runbook provides a standardized procedure for **new employee onboarding** from an IT support perspective.

Effective IT onboarding ensures that new employees receive the appropriate accounts, hardware, software, access permissions, and security configuration required to perform their role from their first day.

The objective of this runbook is to provide IT Support and Service Desk personnel with a repeatable process for preparing technology resources, validating access, documenting assets, and completing the employee's technical onboarding.

---

## Purpose

This procedure is intended to help IT Support personnel:

* Prepare IT resources for new employees
* Create and configure user accounts
* Assign appropriate access
* Prepare laptops and other devices
* Install required applications
* Configure email and collaboration tools
* Configure VPN and network access
* Verify security controls
* Document issued equipment
* Validate access before the employee's start date
* Provide basic technical guidance during onboarding

---

## Scope

This runbook applies to the IT portion of onboarding for:

* New employees
* Contractors
* Interns
* Temporary staff
* Remote employees
* Office-based employees

The exact onboarding process depends on the organization's identity provider, HR system, device management platform, application environment, security policies, and access control procedures.

---

# Onboarding Information

## 1. Confirm the New Employee Request

Obtain the required onboarding information through the organization's approved process.

Confirm:

* [ ] Employee name
* [ ] Start date
* [ ] Job title
* [ ] Department
* [ ] Manager
* [ ] Employment type
* [ ] Work location
* [ ] Remote or office-based status
* [ ] Required applications
* [ ] Required access
* [ ] Required hardware
* [ ] Required peripherals
* [ ] Special technical requirements

Do not proceed with access provisioning without the required authorization.

---

## 2. Confirm Access Requirements

Determine what systems and resources the employee requires based on their role.

Examples include:

* Email
* Microsoft 365 or equivalent productivity platform
* Collaboration applications
* File storage
* Shared drives
* Business applications
* VPN
* Remote desktop
* Departmental applications
* Printers
* Internal websites
* Ticketing systems

Access should follow the organization's **least privilege** and role-based access principles.

Do not grant additional permissions simply for convenience.

---

# User Account Setup

## 3. Create the User Account

Create the employee's organizational account according to the approved identity management process.

Verify:

* [ ] Correct name
* [ ] Correct username
* [ ] Correct email address
* [ ] Correct department
* [ ] Correct manager
* [ ] Correct organizational unit or group
* [ ] Account activation date
* [ ] Appropriate licensing
* [ ] Required security policies

Do not store user passwords in onboarding documentation.

---

## 4. Configure Authentication

Set up the organization's approved authentication methods.

Verify:

* [ ] Password setup process completed
* [ ] MFA enrollment required
* [ ] Authentication methods configured
* [ ] Conditional access policies apply correctly where applicable
* [ ] Recovery procedures are available

Do not collect or record the employee's password or MFA codes.

---

# Licensing

## 5. Assign Required Licenses

Assign licenses based on the employee's role.

Check:

* [ ] Productivity suite license
* [ ] Email license
* [ ] Collaboration tools
* [ ] Business application licenses
* [ ] Security-related licenses
* [ ] Other role-specific software

Verify that licenses are assigned before testing the corresponding applications.

---

# Hardware Preparation

## 6. Assign a Laptop or Desktop

Prepare the employee's primary computer.

Verify:

* [ ] Device selected
* [ ] Asset tag recorded
* [ ] Serial number recorded
* [ ] Device assigned to employee
* [ ] Operating system configured
* [ ] Device management enrollment completed
* [ ] Security baseline applied
* [ ] Windows updates installed

For detailed laptop preparation, follow the [Laptop Setup Checklist](laptop-setup-checklist.md).

---

## 7. Prepare Required Peripherals

Depending on the employee's role, prepare:

* [ ] Monitor
* [ ] Keyboard
* [ ] Mouse
* [ ] Headset
* [ ] Webcam
* [ ] Docking station
* [ ] Power adapter
* [ ] Laptop bag
* [ ] Other approved accessories

Record issued equipment in the asset management system.

---

# Application Setup

## 8. Install Required Applications

Install applications required for the employee's role.

Common examples include:

* Office productivity applications
* Web browsers
* Collaboration tools
* Email applications
* PDF tools
* VPN client
* Remote support software
* Security applications
* Business-specific applications

Use approved software distribution methods wherever available.

---

## 9. Verify Application Access

Test each critical application.

Verify:

* [ ] Application launches
* [ ] User can authenticate
* [ ] License is active
* [ ] Required resources are accessible
* [ ] User has appropriate permissions

Prioritize applications required for the employee's first day.

---

# Email and Collaboration

## 10. Configure Email

Verify:

* [ ] Email account exists
* [ ] User can authenticate
* [ ] Mailbox is accessible
* [ ] Calendar is available
* [ ] Required email policies apply
* [ ] Mobile or secondary access is configured where authorized

Send an approved test message where appropriate.

---

## 11. Configure Collaboration Tools

Set up required collaboration platforms.

Examples include:

* Microsoft Teams
* Slack
* Zoom
* Other organization-approved collaboration applications

Verify that the employee can:

* Sign in
* Access required teams or channels
* Communicate with colleagues
* Join meetings
* Use required audio/video features

---

# Network Access

## 12. Verify Office Network Access

For office-based employees, verify access to:

* Corporate Wi-Fi
* Wired network where applicable
* Internal applications
* Network drives
* Printers
* Other required network resources

Follow the [Wi-Fi Connectivity](wi-fi-connectivity.md) runbook if wireless connectivity problems occur.

---

## 13. Configure Remote Access

For remote employees or employees requiring remote access:

* [ ] VPN client installed
* [ ] VPN profile configured
* [ ] Authentication works
* [ ] MFA works
* [ ] Internal resources accessible
* [ ] Remote access policies applied

Follow the [VPN Troubleshooting](vpn-troubleshooting.md) runbook for VPN problems.

---

# File and Data Access

## 14. Configure Required File Access

Verify access to approved:

* Shared drives
* SharePoint sites
* Cloud storage
* Departmental folders
* Business repositories
* Project resources

Confirm that access matches the employee's role.

Avoid granting broad access where a more limited permission is appropriate.

---

# Printing

## 15. Configure Required Printers

If printing is required:

* [ ] Appropriate printer installed
* [ ] Correct printer selected
* [ ] Test page printed
* [ ] User has required permissions

For printer problems, follow the [Printer Troubleshooting](printer-troubleshooting.md) runbook.

---

# Security Configuration

## 16. Verify Security Controls

Confirm that required security controls are active.

Check:

* [ ] Endpoint protection
* [ ] Firewall
* [ ] Device encryption
* [ ] Secure Boot where required
* [ ] Device management
* [ ] Security policies
* [ ] MFA
* [ ] Screen lock policy
* [ ] Software update policy
* [ ] Compliance status

Do not bypass security controls to complete onboarding faster.

---

# Access Verification

## 17. Perform an Access Review

Before the employee's start date, verify that the employee can access the systems required for their role.

Use an onboarding access checklist covering:

| Resource                  |   Required | Provisioned | Tested |
| ------------------------- | ---------: | ----------: | -----: |
| User account              |        Yes |         [ ] |    [ ] |
| Email                     |        Yes |         [ ] |    [ ] |
| MFA                       |        Yes |         [ ] |    [ ] |
| Productivity applications | Role-based |         [ ] |    [ ] |
| Collaboration tools       | Role-based |         [ ] |    [ ] |
| VPN                       | Role-based |         [ ] |    [ ] |
| File storage              | Role-based |         [ ] |    [ ] |
| Shared drives             | Role-based |         [ ] |    [ ] |
| Business applications     | Role-based |         [ ] |    [ ] |
| Printers                  | Role-based |         [ ] |    [ ] |

---

# First-Day IT Support

## 18. Verify User Sign-In

On or before the employee's first day, verify that the employee can:

* Sign in to the computer
* Authenticate to organizational services
* Complete MFA
* Access email
* Access required applications
* Connect to the required network
* Access required files

---

## 19. Provide Basic Technical Guidance

Explain:

* How to contact IT Support
* How to report technical issues
* How to reset a password using approved methods
* How to connect to Wi-Fi
* How to use VPN
* How to access company applications
* How to lock the computer
* How to protect company information
* How to recognize and report suspicious activity

Provide only the guidance relevant to the employee's environment.

---

# Asset Management

## 20. Update Asset Records

Record issued equipment in the organization's asset management system.

Document:

* Asset tag
* Serial number
* Device type
* Device model
* Assigned employee
* Department
* Assignment date
* Accessories issued
* Device status

Ensure the asset record matches the physical equipment provided to the employee.

---

# Onboarding Validation

## 21. Complete the Final Technical Check

Before closing the onboarding request, confirm:

* [ ] User account active
* [ ] Correct licenses assigned
* [ ] MFA configured
* [ ] Laptop or desktop assigned
* [ ] Device management enrollment completed
* [ ] Security controls active
* [ ] Required applications installed
* [ ] Email working
* [ ] Collaboration tools working
* [ ] Network access working
* [ ] VPN working where required
* [ ] File access working
* [ ] Printers configured where required
* [ ] Required peripherals available
* [ ] Asset records updated
* [ ] User received required IT guidance

---

# Common Onboarding Issues

| Issue                    | Possible Cause                             | Recommended Investigation                              |
| ------------------------ | ------------------------------------------ | ------------------------------------------------------ |
| User cannot sign in      | Account, password, or authentication issue | Verify account status and authentication configuration |
| MFA cannot be completed  | Enrollment or authentication issue         | Verify approved MFA registration process               |
| Email unavailable        | Licensing or mailbox issue                 | Check account licensing and mailbox provisioning       |
| Application unavailable  | Installation, licensing, or permissions    | Verify installation, license, and access               |
| VPN unavailable          | Network, profile, or authentication issue  | Follow VPN troubleshooting procedure                   |
| Wi-Fi unavailable        | Adapter, authentication, or network issue  | Follow Wi-Fi troubleshooting procedure                 |
| Shared drive unavailable | Permissions, network, or VPN issue         | Verify access requirements and connectivity            |
| Printer unavailable      | Configuration or network issue             | Follow printer troubleshooting procedure               |
| Device not compliant     | Policy or enrollment issue                 | Check device management and security policies          |
| Laptop not ready         | Setup or deployment issue                  | Follow laptop setup checklist                          |

---

# Escalation Criteria

Escalate the onboarding request when:

* Required account creation fails
* Identity synchronization fails
* Licensing cannot be assigned
* Required application access cannot be provisioned
* Device enrollment fails
* Security policies cannot be applied
* Hardware is unavailable or defective
* Network access cannot be established
* Business application access requires specialized approval
* Access permissions are unclear
* The requested access exceeds standard role-based permissions
* The issue requires administrative privileges outside the support team's authorization

Provide the escalation team with:

* Employee role
* Start date
* Affected system
* Requested access
* Error messages
* Troubleshooting performed
* Current provisioning status
* Business impact
* Required completion date

---

# Documentation Requirements

Record the following information in the onboarding request:

* Employee name
* Start date
* Department
* Manager
* Role
* Assigned hardware
* Asset information
* Accounts created
* Licenses assigned
* Applications installed
* Access granted
* Testing completed
* Outstanding issues
* Escalations
* Completion status

Do not record passwords, MFA codes, authentication tokens, recovery keys, or other sensitive credentials.

---

# Prevention and Process Improvement

Organizations can improve employee onboarding by:

* Using standardized onboarding checklists
* Automating account provisioning
* Using role-based access groups
* Maintaining standardized laptop configurations
* Using centralized software deployment
* Automating device enrollment
* Maintaining accurate asset records
* Defining onboarding service-level targets
* Testing access before the employee's start date
* Maintaining documented application requirements by role
* Reviewing recurring onboarding failures
* Using feedback to improve the onboarding process

---

# Completion Criteria

The IT onboarding request can be considered complete when:

* Employee account is active
* Required licenses are assigned
* MFA is configured
* Device is prepared and assigned
* Required security controls are active
* Required applications are installed
* Email and collaboration tools work
* Required network access works
* VPN works where required
* Required files and business resources are accessible
* Required peripherals are available
* Asset records are complete
* User has received basic IT guidance
* No unresolved critical onboarding issues remain

---

# Related Runbooks

* [Laptop Setup Checklist](laptop-setup-checklist.md)
* [Account Lockout](account-lockout.md)
* [Password Reset](password-reset.md)
* [Printer Troubleshooting](printer-troubleshooting.md)
* [VPN Troubleshooting](vpn-troubleshooting.md)
* [Wi-Fi Connectivity](wi-fi-connectivity.md)
* [Incident Escalation](incident-escalation.md)
