# Laptop Setup Checklist

## Overview

This runbook provides a standardized procedure for **laptop setup and configuration** in an IT support environment.

A consistent laptop setup process helps ensure that new or reassigned devices are secure, properly configured, compliant with organizational standards, and ready for productive use.

The objective of this runbook is to provide IT Support and Service Desk personnel with a repeatable process for preparing Windows laptops before handing them over to users.

---

## Purpose

This procedure is intended to help IT Support personnel:

* Prepare new or reassigned laptops
* Verify hardware functionality
* Install and configure Windows
* Apply required updates
* Configure organizational accounts
* Enroll devices in endpoint management
* Install approved applications
* Configure security controls
* Verify network connectivity
* Confirm device compliance
* Prepare the laptop for the user

---

## Scope

This checklist applies to:

* New laptops
* Replacement laptops
* Reassigned laptops
* Refurbished organizational laptops
* Standard Windows business devices

The exact setup process may vary depending on the organization's device management platform, identity provider, security requirements, software standards, and hardware configuration.

---

# Pre-Setup Requirements

## 1. Confirm the Device

Verify:

* [ ] Laptop make and model
* [ ] Serial number
* [ ] Asset tag
* [ ] Device name, if already assigned
* [ ] Power adapter
* [ ] Docking station, if applicable
* [ ] Other required accessories

Record the required asset information according to organizational procedures.

---

## 2. Confirm the User Assignment

Verify:

* [ ] User name
* [ ] Department or team
* [ ] Job role
* [ ] Required applications
* [ ] Required hardware or peripherals
* [ ] Required access permissions
* [ ] Device assignment details

Use the organization's approved asset management or service management system.

Do not record passwords or authentication secrets in setup documentation.

---

# Hardware Inspection

## 3. Inspect the Laptop

Check the physical condition of the device.

Inspect:

* [ ] Screen
* [ ] Keyboard
* [ ] Touchpad
* [ ] Power button
* [ ] USB ports
* [ ] USB-C ports
* [ ] HDMI or display ports
* [ ] Audio ports
* [ ] Webcam
* [ ] Microphone
* [ ] Speakers
* [ ] Charging port
* [ ] Wireless adapter
* [ ] Physical casing
* [ ] Hinges

Document any physical damage before assigning the device.

---

## 4. Verify Power and Battery

Check:

* [ ] AC adapter functions correctly
* [ ] Laptop charges correctly
* [ ] Battery is detected
* [ ] Battery health is acceptable
* [ ] Laptop operates on battery power
* [ ] Charging indicator functions correctly

Escalate battery or charging problems before deployment.

---

# Operating System Setup

## 5. Install or Reset Windows

For a new or reassigned laptop:

1. Start the Windows setup process.
2. Select the required language.
3. Select the appropriate region.
4. Configure keyboard settings.
5. Connect to an approved network.
6. Apply the organization's provisioning process.
7. Complete the required Windows setup.

For reassigned devices, follow the organization's approved device reset, wipe, or reimaging procedure.

Ensure previous user data is removed according to organizational security and data-handling requirements.

---

# Device Naming and Enrollment

## 6. Configure the Device Name

Assign the device name according to the organization's naming convention.

Verify:

* [ ] Device name follows the approved naming standard
* [ ] Device name is unique
* [ ] Device name is documented in the asset system

Avoid manually assigning names that conflict with organizational naming standards.

---

## 7. Enroll the Device in Management

Enroll the laptop in the organization's approved device management platform.

Verify:

* [ ] Device enrollment completed
* [ ] Device appears in management console
* [ ] Correct user assignment
* [ ] Required policies applied
* [ ] Compliance status is reported correctly
* [ ] Configuration profiles are applied

Allow sufficient time for policies and applications to synchronize.

---

# Windows Updates

## 8. Install Windows Updates

Check Windows Update and install approved updates.

Verify:

* [ ] Windows is fully updated
* [ ] Required security updates installed
* [ ] Required driver updates installed
* [ ] No critical update failures
* [ ] Required restart completed

Restart the device when required and check for additional updates afterward.

---

# Driver Configuration

## 9. Verify Device Drivers

Check Device Manager for hardware issues.

Verify drivers for:

* [ ] Chipset
* [ ] Graphics
* [ ] Network adapter
* [ ] Wi-Fi adapter
* [ ] Bluetooth
* [ ] Audio
* [ ] Storage
* [ ] Camera
* [ ] Touchpad
* [ ] Docking station
* [ ] Other required hardware

Check for warning icons or unknown devices.

Use only approved driver sources and organizational deployment procedures.

---

# User Account Configuration

## 10. Configure the User Account

Set up the user's organizational account according to company procedures.

Verify:

* [ ] Correct user account
* [ ] Appropriate authentication method
* [ ] Required MFA enrollment
* [ ] Correct organizational access
* [ ] Correct device-user association

Do not handle or store user passwords outside approved authentication processes.

---

# Security Configuration

## 11. Verify Endpoint Security

Confirm that required security controls are active.

Check:

* [ ] Antivirus/endpoint protection
* [ ] Firewall
* [ ] Endpoint detection and response
* [ ] Device encryption
* [ ] Secure Boot
* [ ] Security policies
* [ ] Screen lock policies
* [ ] Automatic security updates
* [ ] Device compliance

Do not disable security controls to speed up the deployment process.

---

## 12. Verify Device Encryption

Confirm that the organization's required encryption solution is enabled.

For Windows devices using BitLocker, verify the appropriate encryption status.

Where applicable:

* [ ] Encryption enabled
* [ ] Device meets encryption requirements
* [ ] Recovery information is stored according to organizational policy

Never place recovery keys in unsecured documentation or personal notes.

---

# Application Installation

## 13. Install Required Applications

Install the applications required for the user's role.

Common examples include:

* Office productivity applications
* Web browsers
* Collaboration software
* PDF software
* VPN client
* Remote support tools
* Security software
* Business-specific applications
* Communication applications

Verify that all software comes from approved organizational sources.

---

## 14. Verify Application Licensing

Confirm:

* [ ] Required applications are licensed
* [ ] User has the required application permissions
* [ ] Applications activate correctly
* [ ] Business applications are accessible

Avoid using personal accounts or unauthorized licenses for organizational software.

---

# Network Configuration

## 15. Test Wi-Fi

Connect to the appropriate wireless network.

Verify:

* [ ] Wi-Fi adapter works
* [ ] Correct SSID is available
* [ ] Authentication works
* [ ] Device receives an IP address
* [ ] Internet connectivity works
* [ ] Required internal network resources are accessible

For troubleshooting, follow the [Wi-Fi Connectivity](wi-fi-connectivity.md) runbook.

---

## 16. Test Ethernet and Docking Station

If the laptop uses a docking station:

* [ ] Ethernet connection works
* [ ] External displays work
* [ ] USB devices work
* [ ] Docking station firmware is appropriate
* [ ] Laptop charges through the dock where supported

Verify that network connectivity switches correctly when connecting and disconnecting from the dock.

---

## 17. Configure and Test VPN

If remote access is required:

* [ ] Approved VPN client installed
* [ ] Correct VPN profile applied
* [ ] Authentication works
* [ ] MFA works
* [ ] VPN connection established
* [ ] Internal resources accessible

Follow the [VPN Troubleshooting](vpn-troubleshooting.md) runbook if problems occur.

---

# Peripheral Testing

## 18. Test Standard Peripherals

Verify required peripherals:

* [ ] External monitor
* [ ] Keyboard
* [ ] Mouse
* [ ] Headset
* [ ] Webcam
* [ ] Microphone
* [ ] Docking station
* [ ] Printer where required

Test audio input and output if the user's role requires video conferencing.

---

# User Environment

## 19. Configure Required User Settings

Apply approved organizational settings such as:

* Desktop configuration
* Start menu configuration
* Browser settings
* Default applications
* File synchronization
* Corporate bookmarks
* Email configuration
* Collaboration tools
* Network drive mappings where applicable

Avoid unnecessary personal customization during the standard setup process.

---

# Data and Migration

## 20. Migrate User Data

For replacement or reassigned devices, follow the organization's approved data migration process.

Verify:

* [ ] Required files backed up
* [ ] Approved cloud synchronization configured
* [ ] Required business data transferred
* [ ] Previous device data handled securely
* [ ] Data transfer completed successfully

Do not copy sensitive business data to unauthorized storage locations.

---

# Testing

## 21. Perform Functional Testing

Before handing over the laptop, test:

* [ ] Windows login
* [ ] User account
* [ ] Internet access
* [ ] Wi-Fi
* [ ] VPN
* [ ] Required applications
* [ ] Email
* [ ] Collaboration tools
* [ ] Web browser
* [ ] Audio
* [ ] Microphone
* [ ] Webcam
* [ ] External display
* [ ] Docking station
* [ ] File access
* [ ] Required internal resources

Testing should reflect the user's actual role where possible.

---

# Performance and Health Check

## 22. Verify Basic System Health

Check:

* [ ] CPU functioning normally
* [ ] Memory recognized correctly
* [ ] Storage functioning correctly
* [ ] Adequate available disk space
* [ ] No unresolved Device Manager errors
* [ ] No critical Windows errors
* [ ] Battery functioning normally
* [ ] Device temperature appears normal
* [ ] No unexpected performance problems

Resolve or document outstanding issues before deployment.

---

# Final Security Check

## 23. Confirm Security Baseline

Before handover, verify:

* [ ] Device encryption enabled
* [ ] Endpoint protection active
* [ ] Firewall enabled
* [ ] Secure Boot enabled where required
* [ ] Windows updates applied
* [ ] Device management active
* [ ] Security policies applied
* [ ] User has appropriate permissions
* [ ] No unnecessary local administrator access
* [ ] No unauthorized software installed

---

# Asset Documentation

## 24. Update Asset Records

Record the appropriate information in the organization's asset management system.

Document:

* Asset tag
* Serial number
* Device model
* Assigned user
* Department
* Device name
* Deployment date
* Operating system
* Warranty information where applicable
* Accessories issued
* Device status

---

# User Handover

## 25. Prepare the Laptop for the User

Before handover:

* [ ] Laptop cleaned appropriately
* [ ] Power adapter included
* [ ] Required accessories included
* [ ] User account verified
* [ ] Required applications installed
* [ ] Network access tested
* [ ] VPN tested where required
* [ ] Security controls verified
* [ ] Asset record updated

---

## 26. Provide Basic User Guidance

Explain relevant procedures such as:

* How to sign in
* How to connect to Wi-Fi
* How to use VPN
* How to access business applications
* How to contact IT Support
* How to report technical problems
* How to protect company information
* How to lock the device when unattended
* How to handle company equipment

The amount of training should be appropriate to the user's role and organizational requirements.

---

# Final Deployment Checklist

* [ ] Hardware inspected
* [ ] Asset information recorded
* [ ] User assignment confirmed
* [ ] Windows installed or reset
* [ ] Device name configured
* [ ] Device enrolled in management
* [ ] Windows updates installed
* [ ] Drivers verified
* [ ] User account configured
* [ ] MFA configured where required
* [ ] Endpoint security verified
* [ ] Device encryption verified
* [ ] Required applications installed
* [ ] Software licensing verified
* [ ] Wi-Fi tested
* [ ] Ethernet tested where required
* [ ] Docking station tested where required
* [ ] VPN tested where required
* [ ] Peripherals tested
* [ ] User data migrated where required
* [ ] Functional testing completed
* [ ] System health checked
* [ ] Asset records updated
* [ ] Accessories confirmed
* [ ] User handover completed

---

# Troubleshooting During Setup

If problems occur during laptop setup, use the appropriate runbook.

Examples:

* [Wi-Fi Connectivity](wi-fi-connectivity.md)
* [VPN Troubleshooting](vpn-troubleshooting.md)
* [Slow Computer Troubleshooting](slow-computer-troubleshooting.md)
* [Blue Screen of Death (BSOD)](blue-screen-of-death-bsod.md)
* [Account Lockout](account-lockout.md)
* [Password Reset](password-reset.md)

---

# Escalation Criteria

Escalate the setup issue to the appropriate team when:

* Hardware is defective
* Device enrollment fails
* Security policies cannot be applied
* Windows activation fails
* Required applications cannot be installed
* Licensing problems cannot be resolved
* Network access cannot be established
* VPN configuration requires administrative changes
* Device encryption cannot be enabled
* Hardware diagnostics report failures
* The device does not meet organizational security requirements
* The issue requires permissions outside the support team's authorization

Document the problem, troubleshooting performed, error messages, and current device status before escalation.

---

# Completion Criteria

A laptop is ready for deployment when:

* Hardware passes inspection
* Windows is correctly configured
* Required updates are installed
* Device management enrollment is successful
* Security controls are active
* Required applications are installed
* Network connectivity is verified
* VPN works where required
* User authentication works
* Required peripherals function correctly
* Asset records are complete
* No unresolved critical issues remain
* User handover requirements are complete
