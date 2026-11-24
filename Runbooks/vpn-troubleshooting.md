# VPN Troubleshooting

**Summary:** A Virtual Private Network (VPN) gives remote users a secure, encrypted connection to the organization's network. When it fails, people cannot reach internal applications, file shares, email, or remote desktops. This runbook helps you find where the connection is breaking, restore secure access, and confirm it works.

**Environment:** Controlled IT support environment (Windows computers, corporate VPN client or built-in Windows VPN, identity provider with MFA, internal DNS, remote users)

**Related runbooks:** `wi-fi-connectivity.md`, `account-lockout.md`, `password-reset.md`, `slow-computer-troubleshooting.md`, `laptop-setup-checklist.md`, `incident-escalation.md`

> **In plain English:** a VPN is a private tunnel from the user's device to the office network. To work, you need an internet connection, valid sign-in, a working VPN client, and a route from the tunnel to the right internal systems. This runbook checks each of those in turn.

---

## Symptoms

A user may report:

- VPN will not connect, or times out
- VPN rejects their credentials, or keeps asking for them
- MFA fails or the prompt never appears
- VPN disconnects repeatedly
- VPN connects, but internal websites, drives, or remote desktops do not work
- Internal names (hostnames) do not resolve
- VPN client will not open or crashes
- VPN is very slow
- VPN works on one network but not another

---

## Prerequisites

Before starting, make sure you have:

- **Information from the user:** VPN client used, operating system, location, network type (home, hotel, mobile hotspot), exact error message, when it began, and whether it ever worked
- **Verified identity:** follow your organization's procedure before making account-related changes
- **Tools:** Command Prompt or PowerShell, the VPN client and its logs, Event Viewer
- **Access:** local administrator rights on the computer where needed, and read access to account status in your identity system
- **VPN documentation:** gateway address, authentication method, expected profile, and internal test resources (a known internal website or server for testing)
- **Awareness:** know whether the VPN profile is centrally managed (do not change managed settings without authorization)

> **Note:** Never ask for or record passwords, MFA codes, private keys, or tokens.

---

## Troubleshooting / Implementation

Work through these in order. Each step checks one link in the chain.

### 1. Identify the problem and the stage
Record:

- VPN client or service, and version
- Operating system, user location, and network used
- The exact error message or code
- When it started, and whether it worked before
- Whether other users are affected

Find out **where** it fails:

- Starting the client
- Signing in
- Establishing the connection
- Reaching internal resources after connecting
- Staying connected

### 2. Check the basic internet connection
A VPN needs a working internet connection first.

```cmd
ipconfig /all
ping 8.8.8.8
nslookup example.com
```

Confirm the device has a valid IP address and gateway, can reach an external website, and can resolve names. If normal internet is down, fix that first (see `wi-fi-connectivity.md`).

> **Note:** On public or hotel Wi-Fi, a sign-in page (captive portal) may need to be completed in a browser before the VPN can connect. Some public networks also block VPN traffic.

### 3. Check that the VPN gateway is reachable
If you know the VPN gateway address and port (from your VPN documentation), test it:

```powershell
Test-NetConnection -ComputerName <vpn-gateway-address> -Port 443
```

Common ports vary by VPN type (for example, 443 for SSL VPN, and UDP 500 and 4500 for IPsec). Use your VPN documentation. If the user's network blocks the VPN port, try a different approved network, such as a mobile hotspot, to see whether the problem is the network.

### 4. Check the sign-in details
Confirm the user is using the right account and current credentials. Check for:

- A recently changed or expired password
- A locked or disabled account
- A wrong username or VPN profile
- A wrong authentication method

If the user recently changed their password, the VPN client may be using old **saved credentials**. Clear or update them in the client.

### 5. Check MFA
If the VPN uses MFA, find out whether the prompt is reaching the user. Possible problems:

- No prompt appears, or the push notification is not received
- The request expired or was repeatedly rejected
- The user cannot reach their MFA device
- The phone or computer clock is wrong

Follow your organization's identity verification and MFA recovery procedures. **Never bypass MFA** to fix a connection problem.

### 6. Check for account restrictions
The VPN may reject a user whose account is:

- Locked, disabled, or expired
- Not licensed (where licensing is required)
- Not in the required VPN access group
- Blocked by a conditional access policy

If a lockout is suspected, follow `account-lockout.md`.

### 7. Check the VPN client
Confirm the approved client is:

- [ ] Installed and running
- [ ] Up to date where required, and a supported version
- [ ] Using the correct profile
- [ ] Not showing an error
- [ ] Not blocked by security software

If it will not start: restart the application, restart the computer if appropriate, check that the required services are running, and look for application errors.

### 8. Check the VPN profile
Confirm the user is connecting with the right configuration: gateway, profile, authentication method, connection type, certificates, and group assignment.

For the built-in Windows VPN:

```powershell
Get-VpnConnection | Select-Object Name, ServerAddress, TunnelType, AuthenticationMethod, SplitTunneling, ConnectionStatus
```

> **Warning:** Do not change security-sensitive VPN settings unless authorized. If the profile is centrally managed, use your organization's deployment process.

### 9. Check the date, time, and time zone
A wrong clock can break sign-in, certificates, and security protocols.

- [ ] Date, time, and time zone are correct
- [ ] Automatic time sync is on where appropriate

```cmd
w32tm /query /status
```

Correct the time, then retry the connection.

### 10. Check certificates
If the VPN uses client or device certificates, check that the certificate:

- Is present and has not expired
- Comes from the expected authority
- Is available to the right user or computer
- Has a valid certificate chain
- Matches a properly enrolled device

You can view the current user's certificates by running `certmgr.msc`, and the computer's by running `certlm.msc`.

Certificate problems usually need the identity, endpoint management, or infrastructure team.

### 11. Check firewall and security software
Endpoint security can block VPN traffic. Check whether:

- Windows Firewall is on and working normally
- Security software reports a block
- A security policy changed recently
- Another VPN or security application is running at the same time (a second VPN can conflict)

Do **not** permanently disable security controls. Any temporary change must follow policy and be reversed after testing.

### 12. If the VPN connects but internal resources fail
First confirm the client shows an active connection, then look at the network settings:

```cmd
ipconfig /all
```

Look for the VPN adapter, its IP address, DNS servers, and routes.

Test an approved internal resource, such as an internal website, file share, remote desktop host, or business application:

```cmd
nslookup internal-server.example.local
ping internal-server.example.local
```

```powershell
Test-NetConnection -ComputerName internal-server.example.local -Port 443
```

> **Note:** A connected VPN does not mean every internal resource is reachable. Access can depend on DNS, routing, and permissions.

### 13. Check DNS
If internal names do not resolve:

```powershell
Get-DnsClientServerAddress | Select-Object InterfaceAlias, ServerAddresses
Resolve-DnsName internal-server.example.local
```

Check that the VPN provides the organization's internal DNS servers. Possible causes include a DNS server that is unavailable, a split-DNS configuration problem, a VPN profile problem, or a routing problem.

You can also try:

```cmd
ipconfig /flushdns
```

### 14. Check routing
If DNS works but the resource is still unreachable:

```cmd
route print
```

```powershell
Get-NetRoute | Where-Object { $_.DestinationPrefix -like '10.*' -or $_.DestinationPrefix -like '172.*' -or $_.DestinationPrefix -like '192.168.*' }
```

Look for routes to the organization's internal networks. Missing or wrong routes can stop traffic. Also remember that the user's **home network** may use the same address range as the office, which can cause conflicts (for example, both using `192.168.1.x`).

> **Warning:** Do not change production routing unless authorized. Escalate routing problems to the network team.

### 15. If the VPN disconnects frequently
Check:

- **Network stability:** Wi-Fi signal, Ethernet connection, packet loss, ISP problems, mobile hotspot stability
- **Network switching:** moving between Wi-Fi networks, access points, Ethernet, a dock, or a hotspot can break the session
- **Sleep and power settings:** a laptop sleeping or saving power can drop the tunnel
- **Timing pattern:** whether disconnects line up with network changes or idle time

Test the normal connection without the VPN to see whether the network itself is unstable.

### 16. If the VPN is slow, or some sites will not load
Check:

- Signal strength and the user's internet speed
- Whether all traffic goes through the VPN (full tunnel) or only company traffic (split tunnel)
- Whether the problem is only with certain large pages or transfers, which can point to a packet size (MTU) problem

A simple packet-size test (use a gateway or internal address your policy allows):

```cmd
ping -f -l 1400 <internal-server-or-gateway>
```

If larger packets fail but smaller ones work, escalate to the network or VPN team, because MTU settings are usually changed centrally.

### 17. Check VPN client logs and Windows events
VPN clients usually keep diagnostic logs. Look for:

- Authentication failures
- Connection timeouts
- Certificate errors
- DNS errors
- Tunnel setup failures
- Network or configuration errors

For the built-in Windows VPN, check Event Viewer (look at **Windows Logs > Application** for source `RasClient`):

```powershell
Get-WinEvent -FilterHashtable @{LogName='Application'; ProviderName='RasClient'} -MaxEvents 20 |
  Select-Object TimeCreated, Id, Message
```

Record error codes and timestamps for escalation. Do not include sensitive authentication information in tickets.

### 18. Restart or reinstall the client (if approved)
**Restart:**

1. Disconnect from the VPN.
2. Close the application and confirm it has fully exited.
3. Reopen it and reconnect.
4. Restart the computer if needed.

**Reinstall** (only if approved):

1. Confirm reinstalling is an allowed step and record the current configuration.
2. Get the approved installation package.
3. Remove the old client and restart if required.
4. Install the approved client and apply the approved configuration.
5. Reconnect and test internal resources.

Never download VPN software from unofficial sources.

### When to escalate
Use `incident-escalation.md` when:

- Several users cannot connect
- The VPN service or gateway looks unavailable
- Authentication, certificate, or DNS infrastructure is failing
- Configuration, routing, or firewall changes are needed
- A security incident is suspected
- The problem continues after approved troubleshooting
- A critical business service is affected

**Include in the escalation:** affected users and how many, VPN client and version, operating system, profile, error message or code, time of failure, the user's network type, test results, relevant logs, and what you already tried.

### Common VPN scenarios

| Symptom | Possible cause | What to check |
|---|---|---|
| Will not connect | Network, credentials, profile, or VPN service | Internet access, gateway reachability, sign-in, profile |
| Authentication fails | Wrong credentials, account restriction, or MFA | Account status and MFA process |
| Connects but internal sites fail | DNS or routing | Internal DNS, routes, and resource tests |
| Disconnects often | Unstable network, sleep, or network switching | Network stability, power settings, logs |
| Client will not start | Application or service problem | Restart, services, logs |
| Works on one network, not another | Blocked ports or network restrictions | Compare networks, test the gateway port |
| MFA prompt does not appear | Identity or client issue | Identity platform and client behavior |
| Stopped after a password change | Saved old credentials | Update stored credentials |
| Certificate error | Expired, missing, or invalid certificate | Certificate status; escalate |
| Many users cannot connect | VPN infrastructure issue | Check for an outage and escalate |
| Connects but is slow, or some pages fail | Weak network, full tunnel, or MTU | Network quality, tunnel type, packet size |
| Internal addresses clash with home network | Overlapping address ranges | Compare ranges; escalate to the network team |

---

## Resolution

Apply the fix that matches the finding:

| Finding | Typical resolution |
|---|---|
| No internet | Fix the connection first (`wi-fi-connectivity.md`) |
| Old saved credentials | Update or clear them in the client |
| Locked, expired, or disabled account | `account-lockout.md`, `password-reset.md`, or escalate |
| MFA problem | Follow the MFA recovery procedure |
| Wrong profile or outdated client | Apply the approved profile or update the client |
| Wrong date or time | Correct the time and resync |
| Expired or missing certificate | Escalate to the identity or endpoint team |
| Security software blocking | Follow policy; escalate for an approved exception |
| DNS or routing fault | Escalate to the network or infrastructure team |
| Network blocks VPN | Use an approved alternative network; report the restriction |
| Client corrupted | Reinstall the approved client |
| Many users affected | Treat as an incident and escalate |

### Document the ticket
Record:

- Date and time reported, user, VPN client, and operating system
- Network used and symptoms
- Error messages and diagnostic results
- Troubleshooting performed and the resolution
- Verification results
- Escalation details, if any

> **Never** record passwords, MFA codes, private keys, tokens, or other secrets in the ticket.

---

## Verification

- [ ] User identity verified
- [ ] Internet connectivity confirmed
- [ ] VPN client and profile verified
- [ ] Account status checked
- [ ] Credentials checked without recording the password
- [ ] MFA process checked
- [ ] Device date and time verified
- [ ] Certificates checked where applicable
- [ ] Security software considered
- [ ] VPN connection established
- [ ] VPN network settings (address, DNS, routes) verified
- [ ] Internal DNS tested
- [ ] Internal resources tested (website, file share, remote desktop, or application)
- [ ] Connection stays stable for a reasonable time
- [ ] User confirms they can reach what they need
- [ ] Ticket updated, and escalation completed if required

Quick final check:

```powershell
Get-VpnConnection | Select-Object Name, ConnectionStatus
Resolve-DnsName internal-server.example.local
Test-NetConnection -ComputerName internal-server.example.local -Port 443
```

> **In plain English:** the user can connect, stay connected, and open the internal systems they need.

---

## Prevention

Organizations can reduce VPN problems by:

- **Supporting current VPN client versions** and updating them centrally
- **Standardizing and centrally managing VPN profiles** so users don't set them up by hand
- **Monitoring VPN gateways, authentication services, DNS, and network links** so outages are caught early
- **Renewing certificates before they expire**
- **Planning internal address ranges** to avoid clashes with common home networks, where possible
- **Documenting remote access procedures** and VPN architecture
- **Reviewing VPN access permissions regularly**
- **Using MFA** and monitoring for unusual sign-in activity
- **Telling users what to do** when passwords change, such as updating saved VPN credentials
- **Keeping a documented VPN incident procedure**

---

## Lessons Learned

- **Check the internet first.** Many "VPN problems" are really Wi-Fi or ISP problems.
- **Find which stage fails.** Connection, sign-in, and internal access are different problems with different fixes.
- **Connected does not mean working.** DNS and routing often cause "VPN is on but nothing opens".
- **Old saved credentials cause trouble after password changes.** Update them in the client and everywhere else.
- **Clock errors break security.** Always check date, time, and time zone.
- **Test from another network.** If it works on a hotspot, the user's usual network is blocking it.
- **Many users down means an incident.** Stop troubleshooting individual devices and escalate.
- **Never bypass MFA or disable security tools** to get the user connected.

### My notes
_Add your own lessons here from lab exercises or real tickets:_

- 

---

## References

- Microsoft Learn: Windows VPN technical guide and `Get-VpnConnection`
- Microsoft Learn: `Test-NetConnection`, `Resolve-DnsName`, `Get-DnsClientServerAddress`, and `route print`
- Microsoft Learn: Troubleshoot Windows networking, DNS, and time synchronization (`w32tm`)
- Your VPN vendor's documentation (client, gateway, ports, logs, and error codes)
- Your organization's remote access, identity, MFA, and incident response procedures
