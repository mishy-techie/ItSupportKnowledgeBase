# VPN Troubleshooting Runbook

## Overview

This runbook provides a standardized procedure for troubleshooting **VPN connection problems** in an IT support environment.

A Virtual Private Network (VPN) provides an encrypted connection between a user's device and an organization's network or VPN service. VPN problems can prevent users from accessing internal applications, file shares, email services, remote desktops, and other business resources.

The objective of this runbook is to identify the cause of a VPN connectivity issue, restore secure remote access, verify connectivity, and determine when escalation is required.

---

## Purpose

This procedure is intended to help IT Support and Service Desk personnel:

* Diagnose common VPN connection problems
* Identify authentication and connectivity issues
* Troubleshoot VPN client problems
* Verify network connectivity before connecting to a VPN
* Troubleshoot DNS and internal resource access
* Identify configuration and certificate issues
* Verify successful VPN connectivity
* Determine when VPN issues require escalation

---

## Scope

This runbook applies to common VPN troubleshooting scenarios involving:

* Windows VPN clients
* Corporate VPN applications
* Remote users
* VPN authentication
* VPN connectivity
* VPN configuration
* DNS resolution
* Network connectivity
* VPN certificates
* Multi-factor authentication
* Internal network resource access

The exact troubleshooting process depends on the organization's VPN platform, authentication architecture, network design, and security policies.

---

# Common VPN Problems

Users may report:

* VPN will not connect
* VPN connection times out
* VPN credentials are rejected
* VPN repeatedly disconnects
* VPN connects but internal resources are unavailable
* VPN client will not open
* VPN client crashes
* VPN connection is extremely slow
* VPN authentication repeatedly prompts for credentials
* MFA authentication fails
* Internal websites cannot be accessed
* Network drives are unavailable while connected to VPN
* Remote Desktop cannot connect through the VPN
* DNS names do not resolve while connected
* VPN works on one network but not another

---

# Troubleshooting Procedure

## 1. Identify the Problem

Determine exactly what the user is experiencing.

Confirm:

* VPN client or service being used
* User's operating system
* Location of the user
* Network being used
* Exact error message
* Time the problem started
* Whether the VPN worked previously
* Whether other users are experiencing the same problem

Determine whether the issue occurs during:

* VPN client startup
* Authentication
* Connection establishment
* Network resource access
* Active VPN session

---

## 2. Verify the User's Internet Connection

A VPN requires an underlying network connection.

Before troubleshooting the VPN itself, confirm that the user's device has working internet access.

Check:

* Wi-Fi or Ethernet connection
* IP address
* Default gateway
* DNS configuration
* Ability to access a known external website

On Windows, use:

```powershell id="6f24zw"
ipconfig /all
```

Test basic connectivity:

```text id="9dx3u8"
ping 8.8.8.8
```

Test DNS resolution:

```text id="k2z4mg"
nslookup example.com
```

If the user's normal internet connection is unavailable, troubleshoot the underlying network connection before continuing with VPN troubleshooting.

---

## 3. Verify VPN Credentials

Confirm that the user is using the correct account and current credentials.

Check for:

* Recently changed password
* Expired password
* Locked account
* Disabled account
* Incorrect username
* Incorrect VPN profile
* Incorrect authentication method

Do not request or record the user's password.

If the user recently changed their password, ensure that the VPN client is not using outdated stored credentials.

---

## 4. Check Multi-Factor Authentication

If the VPN uses MFA, verify whether authentication is completing successfully.

Possible problems include:

* MFA prompt not appearing
* Incorrect authentication method
* Expired authentication request
* User unable to access their MFA device
* Push notification not received
* Authentication request repeatedly rejected
* Device time synchronization issues

Follow the organization's identity verification and MFA recovery procedures where required.

Never bypass MFA controls to resolve a connectivity problem.

---

## 5. Check VPN Client Status

Verify that the organization's approved VPN client is:

* Installed
* Running
* Updated where required
* Using the correct VPN profile
* Not displaying an error
* Not blocked by another security application

If the VPN client fails to launch:

1. Restart the application.
2. Restart the computer if appropriate.
3. Check whether the required VPN services are running.
4. Check for application errors.
5. Verify that the client version is supported.

---

## 6. Verify the VPN Profile

Confirm that the user is connecting to the correct VPN configuration.

Check:

* VPN server or gateway
* VPN profile
* Authentication method
* Connection type
* Required certificates
* User group or access assignment
* Approved configuration settings

Do not manually change security-sensitive VPN settings unless authorized.

If the VPN profile is centrally managed, follow the organization's deployment and configuration procedures.

---

## 7. Check for Account Lockout or Access Restrictions

A VPN may reject authentication if the user's account is:

* Locked
* Disabled
* Expired
* Not licensed where licensing is required
* Not assigned to the required VPN access group
* Restricted by conditional access policies

If an account lockout is suspected, follow the appropriate account lockout troubleshooting procedure.

---

## 8. Check Device Date and Time

Incorrect system time can interfere with authentication, certificates, and security protocols.

Verify that:

* Date is correct
* Time is correct
* Time zone is correct
* Automatic time synchronization is enabled where appropriate

After correcting the device time, retry the VPN connection.

---

## 9. Check VPN Certificates

Some VPN implementations require client or device certificates.

If certificates are used, verify:

* Certificate is present
* Certificate has not expired
* Certificate is issued by the expected authority
* Certificate is available to the required user or computer
* Certificate chain is valid
* Device is correctly enrolled

Certificate-related issues may require escalation to the identity, endpoint management, or infrastructure team.

---

## 10. Check Firewall and Security Software

Endpoint security software may interfere with VPN connections in some environments.

Check whether:

* Windows Firewall is enabled and functioning normally
* Endpoint security software reports a block
* Recent security policy changes occurred
* VPN traffic is being blocked
* Another VPN or security application is active

Do not permanently disable security controls as a troubleshooting shortcut.

Any temporary security-related changes must follow organizational policy and be reversed after testing.

---

# VPN Connects but Internal Resources Are Unavailable

## 11. Verify VPN Connection Status

Once connected, verify that the VPN client reports an active connection.

Check the device's network configuration:

```powershell id="0j6v5u"
ipconfig /all
```

Look for:

* VPN adapter
* Assigned VPN IP address
* DNS servers
* Routes
* Default gateway

The exact output varies by VPN architecture.

---

## 12. Test Internal Connectivity

Test access to an approved internal resource.

Examples include:

* Internal website
* File share
* Internal DNS name
* Remote Desktop host
* Business application

Test DNS resolution:

```text id="3xv8ul"
nslookup internal-server.example.local
```

Test connectivity where ICMP is permitted:

```text id="bgr7wq"
ping internal-server.example.local
```

A successful VPN connection does not necessarily mean that every internal resource is reachable.

---

## 13. Check DNS Resolution

If the VPN connects but internal hostnames do not resolve, DNS configuration may be incorrect.

Compare DNS behavior:

```text id="h0odtv"
nslookup internal-server.example.local
```

Check whether the VPN connection provides the organization's internal DNS servers.

Possible causes include:

* Incorrect DNS configuration
* DNS server unavailable
* Split-DNS configuration issue
* VPN profile problem
* Network routing issue

Escalate DNS or VPN infrastructure problems when they cannot be resolved at the support level.

---

## 14. Check Routing

If DNS resolution works but internal resources remain unreachable, routing may be involved.

On Windows, inspect the routing table:

```text id="h5v4dr"
route print
```

Look for routes associated with the organization's internal networks.

Incorrect or missing routes can prevent traffic from reaching internal resources.

Do not manually modify production routing configuration unless authorized.

---

# VPN Disconnects Frequently

## 15. Check Network Stability

Frequent VPN disconnections may be caused by an unstable underlying network.

Check:

* Wi-Fi signal strength
* Ethernet connection
* Packet loss
* Network switching
* Mobile hotspot stability
* ISP connectivity
* Device power-saving settings

Test the user's normal network connection independently of the VPN.

---

## 16. Check Sleep and Power Settings

A laptop entering sleep or aggressive power-saving states can interrupt VPN connectivity.

Check appropriate device power settings and organizational endpoint policies.

If the issue occurs consistently when the device is idle, investigate power management settings.

---

## 17. Check for Network Switching

A device may switch between:

* Wi-Fi networks
* Ethernet
* Mobile hotspot
* Docking station network
* Different wireless access points

Network changes can interrupt an active VPN session.

Determine whether disconnections correspond with changes in network connectivity.

---

# VPN Client Troubleshooting

## 18. Restart the VPN Client

If the VPN client appears stuck:

1. Disconnect from the VPN.
2. Close the VPN application.
3. Confirm that the application has fully exited.
4. Reopen the VPN client.
5. Attempt to reconnect.

If the problem persists, restart the computer where appropriate.

---

## 19. Check VPN Client Services

Some VPN applications depend on background Windows services.

Verify the relevant services are running.

Where the service name is known:

```powershell id="1w9k4c"
Get-Service -Name "<service-name>"
```

Restart only services that are approved for troubleshooting and within the technician's authorization.

---

## 20. Check VPN Client Logs

VPN clients commonly provide diagnostic logs.

Check logs for:

* Authentication failures
* Connection timeouts
* Certificate errors
* DNS errors
* Tunnel establishment failures
* Network errors
* Configuration problems

Record relevant error codes and timestamps for escalation.

Do not include sensitive authentication information in support tickets.

---

# Reinstalling the VPN Client

If the VPN client is suspected to be corrupted:

1. Confirm that reinstalling the client is an approved troubleshooting step.
2. Record the existing configuration if required.
3. Obtain the approved installation package.
4. Remove the existing client if appropriate.
5. Restart the device if required.
6. Install the approved VPN client.
7. Apply the organization's approved configuration.
8. Reconnect to the VPN.
9. Test access to internal resources.

Do not download VPN software from unofficial sources.

---

# Common VPN Scenarios

| Symptom                                   | Possible Cause                                           | Recommended Investigation                                          |
| ----------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------------ |
| VPN will not connect                      | Network, credentials, profile, or VPN service            | Check internet connectivity, authentication, and VPN configuration |
| Authentication fails                      | Incorrect credentials, account restriction, or MFA issue | Verify account status and authentication process                   |
| VPN connects but internal sites fail      | DNS or routing issue                                     | Test internal DNS and connectivity                                 |
| VPN disconnects frequently                | Unstable network or client issue                         | Check network stability and VPN logs                               |
| VPN client will not start                 | Application or service issue                             | Restart client, check services, and review logs                    |
| VPN works on one network but not another  | Network restrictions or firewall                         | Compare network environments                                       |
| MFA prompt does not appear                | Authentication or client issue                           | Verify identity platform and VPN client behavior                   |
| VPN stopped working after password change | Cached credentials                                       | Update stored credentials and reconnect                            |
| Certificate error                         | Expired, missing, or invalid certificate                 | Check certificate status and escalate if necessary                 |
| Multiple users cannot connect             | VPN infrastructure issue                                 | Check for service outage and escalate                              |

---

# Verification Checklist

* [ ] User identity verified
* [ ] Internet connectivity confirmed
* [ ] VPN client identified
* [ ] VPN profile verified
* [ ] Account status checked
* [ ] Credentials verified without recording the password
* [ ] MFA process checked
* [ ] Device date and time verified
* [ ] VPN client status checked
* [ ] Required certificates checked where applicable
* [ ] Security software considered
* [ ] VPN connection established
* [ ] VPN-assigned network configuration verified
* [ ] Internal DNS tested where applicable
* [ ] Internal resource connectivity tested
* [ ] User confirms required services are accessible
* [ ] Ticket updated with troubleshooting performed
* [ ] Escalation completed if required

---

# Escalation Criteria

Escalate the issue to the appropriate network, infrastructure, identity, security, or VPN administration team when:

* Multiple users cannot connect to the VPN
* The VPN service appears unavailable
* VPN gateway infrastructure is unreachable
* Authentication infrastructure is failing
* Certificate infrastructure is affected
* VPN configuration requires administrative changes
* Routing or firewall changes are required
* DNS infrastructure is unavailable
* A suspected security incident is identified
* The issue persists after approved troubleshooting
* The problem affects a critical business service

Provide the escalation team with:

* Affected user or users
* VPN client and version
* Operating system
* VPN profile
* Error message or error code
* Time of failure
* User's network type
* Troubleshooting performed
* VPN logs where appropriate
* Connectivity test results
* Number of affected users

---

# Documentation Requirements

Record the following information in the support ticket:

* Date and time reported
* Affected user
* VPN service or client
* Operating system
* Network being used
* Reported symptoms
* Error messages
* Troubleshooting steps performed
* Relevant diagnostic results
* Resolution
* Verification results
* Escalation details where applicable

Never record passwords, MFA codes, private keys, authentication tokens, or other secrets.

---

# Prevention

Organizations can reduce recurring VPN issues by:

* Maintaining supported VPN client versions
* Standardizing VPN configurations
* Monitoring VPN infrastructure
* Maintaining accurate VPN documentation
* Monitoring authentication services
* Maintaining valid certificates
* Monitoring DNS and network infrastructure
* Providing clear remote-access procedures
* Regularly reviewing VPN access permissions
* Implementing appropriate MFA
* Monitoring unusual authentication activity
* Maintaining documented VPN incident procedures

---

# Related Runbooks

* [Account Lockout](account-lockout.md)
* [Password Reset](password-reset.md)
* [Wi-Fi Connectivity](wi-fi-connectivity.md)
* [Incident Escalation](incident-escalation.md)
