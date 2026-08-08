# Wi-Fi Connectivity Troubleshooting Runbook

## Overview

This runbook provides a standardized procedure for troubleshooting **Wi-Fi connectivity problems** in an IT support environment.

Wireless connectivity issues can prevent users from accessing the internet, corporate applications, VPN services, network drives, printers, and other business resources.

The objective of this runbook is to identify whether the problem originates from the user's device, wireless network, authentication, DNS, DHCP, access point, or wider network infrastructure, then restore connectivity and verify the resolution.

---

## Purpose

This procedure is intended to help IT Support and Service Desk personnel:

* Diagnose common Wi-Fi connectivity problems
* Troubleshoot Windows wireless network issues
* Identify authentication and password problems
* Diagnose IP address and DHCP issues
* Troubleshoot DNS-related connectivity problems
* Identify weak or unstable wireless connections
* Distinguish device-specific problems from network-wide outages
* Verify successful network connectivity
* Determine when escalation is required

---

## Scope

This runbook applies to common wireless networking scenarios involving:

* Windows laptops and desktop computers with Wi-Fi adapters
* Corporate wireless networks
* Home Wi-Fi networks used for remote work
* Guest wireless networks
* Wireless authentication
* DHCP
* DNS
* Wireless access points
* Network connectivity
* VPN connectivity over Wi-Fi

The exact troubleshooting procedure may vary depending on the organization's wireless infrastructure, authentication method, network segmentation, and endpoint management solution.

---

# Common Wi-Fi Problems

Users may report:

* Cannot connect to Wi-Fi
* Wi-Fi network is not visible
* Wi-Fi keeps disconnecting
* Wi-Fi is connected but there is no internet
* Wi-Fi is extremely slow
* Connected to the wrong wireless network
* "No Internet" or "No Internet, secured"
* Unable to obtain an IP address
* Wi-Fi password is rejected
* Corporate Wi-Fi authentication fails
* VPN disconnects while using Wi-Fi
* Network drives or internal applications are unavailable
* Wi-Fi works on one device but not another

---

# Troubleshooting Procedure

## 1. Identify the Problem

Determine exactly what the user is experiencing.

Confirm:

* User's device
* Operating system
* Wireless network name (SSID)
* User's location
* Whether the problem is occurring at the office, home, or another location
* Approximate time the problem started
* Whether the device connected successfully before
* Whether other devices are experiencing the same issue
* Whether the problem affects internet access, internal resources, or both

Determine whether the problem occurs during:

* Detecting the wireless network
* Connecting to the wireless network
* Authentication
* Obtaining an IP address
* Accessing network resources
* Maintaining the connection

---

## 2. Determine the Scope of the Problem

Before changing settings, establish whether the issue affects one device or multiple devices.

### If only one device is affected

Possible causes include:

* Wi-Fi adapter
* Device configuration
* Driver
* Saved wireless profile
* Authentication credentials
* Device security software
* Local network settings

### If multiple devices are affected

Possible causes include:

* Wireless access point
* Network infrastructure
* DHCP
* DNS
* Authentication service
* Network outage
* Internet service interruption

This distinction helps determine whether the issue should be handled as an endpoint problem or a wider network incident.

---

# Basic Wi-Fi Checks

## 3. Verify Wi-Fi Is Enabled

Check that:

* Wi-Fi is enabled
* Airplane mode is disabled
* Wireless adapter is enabled
* Device is within reasonable wireless range
* The expected wireless network is visible

On laptops, check whether a physical wireless switch or function key has disabled the wireless adapter.

---

## 4. Confirm the Correct Wireless Network

Verify that the device is connected to the intended SSID.

Be particularly careful when multiple networks are available, such as:

* Corporate Wi-Fi
* Guest Wi-Fi
* Personal hotspot
* Home Wi-Fi
* Neighboring networks

A user may appear connected but have no access to required corporate resources because the device is connected to the wrong network.

---

## 5. Check Wireless Signal Strength

Check the Wi-Fi signal strength.

A weak signal can cause:

* Slow connections
* Intermittent connectivity
* Frequent disconnections
* High latency
* Poor VPN performance

If signal strength is poor:

1. Move closer to the wireless access point where practical.
2. Test the connection again.
3. Determine whether the issue follows the user's location.
4. Check whether other users experience similar problems in the same area.

If problems consistently occur in a particular physical location, the issue may require wireless infrastructure investigation.

---

# Windows Network Configuration

## 6. Check the Network Adapter

Verify that Windows detects the wireless adapter.

Check:

* Adapter is enabled
* Adapter appears in Device Manager
* No hardware errors are displayed
* Correct driver is installed
* Adapter is not repeatedly disconnecting

If the adapter has an error, investigate the associated Windows device or driver issue.

---

## 7. Check the IP Configuration

Open Command Prompt or PowerShell and run:

```text id="9znf0a"
ipconfig /all
```

Review:

* IPv4 address
* Subnet mask
* Default gateway
* DNS servers
* DHCP status
* Wireless adapter information

A valid address from the expected network generally indicates that DHCP has successfully provided network configuration.

An address beginning with:

```text id="lypgc7"
169.254.x.x
```

may indicate that the device was unable to obtain an IPv4 address from DHCP.

---

## 8. Renew the IP Address

If DHCP appears to be the problem, renew the network configuration.

Run:

```text id="0kvlc8"
ipconfig /release
```

Then:

```text id="rq8yvb"
ipconfig /renew
```

Verify the resulting configuration:

```text id="p7wq5d"
ipconfig /all
```

If the device cannot obtain a valid address, investigate DHCP availability and wireless network connectivity.

---

# Connectivity Testing

## 9. Test the Default Gateway

Test connectivity to the assigned gateway:

```text id="a4yq3u"
ping <default-gateway>
```

A successful response indicates that the device can communicate with the gateway, assuming ICMP is permitted.

A failed test may indicate:

* Weak wireless connection
* Incorrect IP configuration
* Wireless isolation
* Access point problem
* Network configuration issue

---

## 10. Test Internet Connectivity

Test an external IP address:

```text id="1g4yxx"
ping 8.8.8.8
```

If the gateway responds but the external IP does not, investigate:

* Routing
* Firewall
* Internet connectivity
* Network outage

ICMP may be blocked by some networks, so a failed ping does not by itself prove that internet access is unavailable.

---

## 11. Test DNS Resolution

Test DNS using:

```text id="9r8cwy"
nslookup example.com
```

If external IP connectivity works but DNS resolution fails, investigate DNS configuration.

Check:

* DNS server addresses
* DNS availability
* VPN configuration
* Network-provided DNS settings
* Organizational DNS policies

---

# Wi-Fi Authentication Troubleshooting

## 12. Verify Wireless Credentials

For networks requiring user authentication, confirm that the user is authenticating with the expected account and current credentials.

Possible causes include:

* Incorrect password
* Recently changed password
* Locked account
* Expired password
* Incorrect username
* Incorrect authentication method
* Device not authorized for the network

Do not request or record passwords.

If an account lockout is suspected, follow the [Account Lockout](account-lockout.md) runbook.

---

## 13. Check Enterprise Wi-Fi Authentication

Corporate Wi-Fi may use technologies such as:

* WPA2-Enterprise
* WPA3-Enterprise
* 802.1X
* RADIUS
* Certificate-based authentication

Authentication failures may be caused by:

* Invalid credentials
* Expired certificate
* Missing certificate
* Incorrect wireless profile
* Device not registered
* Authentication server unavailable
* Incorrect network policy

Issues involving enterprise authentication infrastructure may require escalation.

---

# Forget and Reconnect to Wi-Fi

## 14. Remove a Problematic Wireless Profile

If the saved wireless configuration appears corrupted or contains outdated credentials, remove the saved network and reconnect.

In Windows:

1. Open Wi-Fi settings.
2. Locate the affected network.
3. Select the option to forget the network.
4. Confirm the network is removed.
5. Reconnect to the correct SSID.
6. Authenticate using the current approved credentials.
7. Test connectivity.

Do not remove corporate wireless profiles managed by organizational policy unless authorized.

---

# Wi-Fi Driver Troubleshooting

## 15. Check the Wireless Driver

An outdated, corrupted, or incompatible wireless driver can cause:

* Frequent disconnections
* Missing Wi-Fi networks
* Slow wireless performance
* Adapter crashes
* Intermittent connectivity

Check the wireless adapter in Device Manager.

Verify:

* Device status
* Driver version
* Driver date
* Error codes
* Recent driver changes

Use only approved driver packages and organizational deployment procedures when updating drivers.

---

## 16. Reinstall or Update the Wireless Driver

If the wireless driver is suspected:

1. Identify the exact wireless adapter.
2. Record the current driver information.
3. Obtain the approved driver.
4. Update or reinstall the driver.
5. Restart the device if required.
6. Reconnect to the wireless network.
7. Test connectivity.

If the issue began immediately after a driver update, investigate whether an approved rollback is appropriate.

---

# Wi-Fi Performance Problems

## 17. Investigate Slow Wi-Fi

Slow wireless performance can have several causes.

Check:

* Signal strength
* Distance from access point
* Number of connected devices
* Network congestion
* VPN usage
* Background downloads
* Device performance
* Internet service performance

Test the connection with and without VPN where organizational policy permits.

If performance problems occur only in a specific area, investigate the wireless infrastructure serving that location.

---

## 18. Check for Intermittent Disconnections

If Wi-Fi repeatedly disconnects:

1. Determine whether the issue occurs at a specific location.
2. Check signal strength.
3. Check whether other devices are affected.
4. Review adapter and driver status.
5. Check for network switching.
6. Test with another approved wireless network where appropriate.
7. Check whether the device enters sleep or power-saving states.
8. Record the frequency and timing of disconnections.

Frequent disconnections affecting multiple users should be treated as a potential wireless infrastructure issue.

---

# VPN Over Wi-Fi

If Wi-Fi works normally but VPN connectivity is unstable:

1. Verify normal internet access.
2. Confirm Wi-Fi stability.
3. Test the connection without VPN where appropriate.
4. Check VPN client status.
5. Check for packet loss or network instability.
6. Follow the [VPN Troubleshooting](vpn-troubleshooting.md) runbook.

Do not assume that a VPN problem is caused by Wi-Fi simply because the user is connected wirelessly.

---

# Common Wi-Fi Scenarios

| Symptom                                    | Possible Cause                                       | Recommended Investigation                                       |
| ------------------------------------------ | ---------------------------------------------------- | --------------------------------------------------------------- |
| Wi-Fi network is not visible               | Adapter, signal, access point, or configuration      | Check adapter, location, and available networks                 |
| Wi-Fi will not connect                     | Credentials, profile, or authentication              | Verify SSID, credentials, and wireless profile                  |
| Connected but no internet                  | Gateway, DNS, routing, or internet outage            | Check IP configuration, gateway, DNS, and external connectivity |
| 169.254.x.x address                        | DHCP failure                                         | Renew IP configuration and investigate DHCP                     |
| Wi-Fi keeps disconnecting                  | Weak signal, driver, interference, or infrastructure | Check signal, driver, location, and other affected users        |
| Wi-Fi is slow                              | Signal, congestion, bandwidth, or device issue       | Check signal, network utilization, and device performance       |
| Only one user is affected                  | Endpoint configuration or adapter                    | Troubleshoot the affected device                                |
| Multiple users are affected                | Access point or network infrastructure               | Check for wider network incident                                |
| Corporate Wi-Fi rejects credentials        | Authentication or account issue                      | Check account status and enterprise Wi-Fi configuration         |
| Wi-Fi works but VPN fails                  | VPN or authentication issue                          | Follow VPN troubleshooting procedure                            |
| Internet works but internal resources fail | DNS, routing, or network segmentation                | Check internal DNS and network access                           |

---

# Verification Checklist

* [ ] User's symptoms identified
* [ ] Scope of the issue determined
* [ ] Wi-Fi enabled
* [ ] Correct SSID confirmed
* [ ] Signal strength checked
* [ ] Wireless adapter checked
* [ ] Wireless driver checked
* [ ] IP configuration reviewed
* [ ] DHCP status checked
* [ ] Default gateway tested
* [ ] Internet connectivity tested
* [ ] DNS resolution tested
* [ ] Wireless authentication checked
* [ ] Wireless profile reviewed where appropriate
* [ ] VPN impact checked where applicable
* [ ] Test connection completed
* [ ] User confirms required connectivity
* [ ] Ticket updated
* [ ] Escalation completed if required

---

# Escalation Criteria

Escalate the issue to the appropriate network, infrastructure, identity, or security team when:

* Multiple users cannot connect to the same Wi-Fi network
* An entire area has lost wireless connectivity
* Multiple access points appear unavailable
* DHCP services appear unavailable
* DNS infrastructure is unavailable
* Enterprise wireless authentication is failing
* Network configuration changes are required
* Wireless infrastructure requires physical maintenance
* The issue persists after approved endpoint troubleshooting
* There is evidence of unauthorized wireless activity
* The problem affects a critical business service

Provide the escalation team with:

* Affected user or users
* Device information
* Wireless network name
* User location
* Approximate time the problem started
* IP configuration
* Connectivity test results
* Error messages
* Wireless signal information where available
* Troubleshooting performed
* Number of affected devices

---

# Documentation Requirements

Record the following information in the support ticket:

* Date and time reported
* Affected user
* Device
* Wireless network
* User location
* Symptoms
* Error messages
* IP configuration findings where relevant
* Connectivity test results
* Troubleshooting performed
* Changes made
* Resolution
* Verification results
* Escalation details where applicable

Do not record passwords, MFA codes, authentication tokens, or other sensitive credentials.

---

# Prevention

Organizations can reduce recurring Wi-Fi connectivity problems by:

* Maintaining current wireless infrastructure
* Monitoring access points
* Maintaining appropriate wireless coverage
* Standardizing supported wireless configurations
* Keeping wireless drivers current
* Monitoring DHCP and DNS services
* Maintaining enterprise wireless authentication infrastructure
* Documenting wireless network architecture
* Monitoring network performance
* Reviewing wireless capacity
* Maintaining appropriate network segmentation
* Providing users with approved wireless troubleshooting procedures

---

# Related Runbooks

* [VPN Troubleshooting](vpn-troubleshooting.md)
* [Account Lockout](account-lockout.md)
* [Password Reset](password-reset.md)
* [Incident Escalation](incident-escalation.md)
