# Wi-Fi Connectivity

**Summary:** Wi-Fi problems stop people from reaching the internet, company applications, VPN, printers, and shared drives. This runbook helps you work out whether the problem is with the user's device, the wireless signal, sign-in, IP address (DHCP), name lookup (DNS), or the wider network, then fix it and confirm it worked.

**Environment:** Controlled IT support environment (Windows laptops and desktops with Wi-Fi adapters, corporate, guest, and home wireless networks, endpoint management)

**Related runbooks:** `vpn-troubleshooting.md`, `account-lockout.md`, `password-reset.md`, `laptop-setup-checklist.md`, `incident-escalation.md`

> **In plain English:** getting online is a chain of steps: find the network, sign in, get an address, reach the internet, and look up website names. This runbook tests each link in the chain to find the one that is broken.

---

## Symptoms

A user may report:

- Cannot connect, or the Wi-Fi network is not visible
- Wi-Fi keeps disconnecting
- Connected but "No Internet" or "No Internet, secured"
- Wi-Fi is very slow
- Connected to the wrong network
- Password rejected, or corporate Wi-Fi sign-in fails
- Cannot get an IP address
- VPN drops while on Wi-Fi
- Network drives or internal apps are unavailable
- Works on one device but not another

---

## Prerequisites

Before starting, make sure you have:

- **Information from the user:** device, location (office, home, or elsewhere), network name (SSID), when it started, whether it worked before, and whether others are affected
- **Tools:** Command Prompt or PowerShell, Device Manager, and Windows Wi-Fi settings
- **Access:** local administrator rights on the device for adapter and driver changes (where authorized)
- **Approved sources:** your organization's approved wireless drivers and profiles
- **Awareness:** know which networks are managed by company policy (do not remove corporate profiles without authorization)

> **Note:** Do not ask for or record Wi-Fi passwords, MFA codes, or tokens.

---

## Troubleshooting / Implementation

Work through these in order. Each step tests one link in the chain.

### 1. Identify the problem
Find out which stage is failing:

- Seeing the network
- Connecting to it
- Signing in (authentication)
- Getting an IP address
- Reaching internal resources or the internet
- Staying connected

### 2. Check the scope
Decide whether this is **one device** or **many**.

| Scope | Likely areas |
|---|---|
| **One device** | Adapter, driver, saved Wi-Fi profile, credentials, security software, local settings |
| **Many devices** | Access point, DHCP, DNS, authentication service, network or internet outage |

Many affected users means a possible wider incident. Go to **When to escalate**.

### 3. Check the basics
- [ ] Wi-Fi is turned on and **Airplane mode** is off
- [ ] Wireless adapter is enabled
- [ ] Physical Wi-Fi switch or function key is not disabling it
- [ ] The user is within range and the expected network is visible
- [ ] Restarting the device has been tried

### 4. Confirm the correct network
Check the user is on the **right SSID**. A device on guest Wi-Fi, a phone hotspot, or a neighbor's network may be online but unable to reach company resources.

On guest networks, check whether a sign-in or terms page (captive portal) must be completed in a browser.

### 5. Check signal strength
Weak signal causes slow speeds, drops, and poor VPN performance. View the current connection details:

```cmd
netsh wlan show interfaces
```

Check **SSID**, **Signal**, **Radio type**, and **Channel**. If the signal is poor:

1. Move closer to the access point and test again.
2. See whether the problem follows the user or stays in one location.
3. Ask whether others in the same area are affected.

A problem in one physical area points to the wireless infrastructure.

### 6. Check the adapter
```powershell
Get-NetAdapter | Where-Object { $_.Name -like '*Wi-Fi*' -or $_.InterfaceDescription -like '*Wireless*' }
```

Also open **Device Manager** and check the adapter is present, enabled, error-free, and using the right driver.

### 7. Check the IP configuration
```cmd
ipconfig /all
```

Look at the IPv4 address, subnet mask, default gateway, DNS servers, and DHCP status.

- **A valid address on the expected network** means DHCP worked.
- **An address starting with `169.254`** means the device could not get an address from DHCP.

### 8. Renew the IP address
```cmd
ipconfig /release
ipconfig /renew
ipconfig /all
```

If it still cannot get a valid address, suspect DHCP or the wireless connection itself.

### 9. Test the connection layer by layer

| Test | Command | What a failure suggests |
|---|---|---|
| Gateway | `ping <default-gateway>` | Weak Wi-Fi, bad IP settings, wireless isolation, or an access point problem |
| Internet (by IP) | `ping 8.8.8.8` | Routing, firewall, internet outage |
| Name lookup (DNS) | `nslookup example.com` | DNS server or configuration problem |

Newer alternatives:

```powershell
Test-NetConnection -ComputerName example.com -Port 443
Resolve-DnsName example.com
```

> **Notes:**
> - Some networks block ping, so a failed ping alone does not prove there is no internet.
> - If the IP test works but the name lookup fails, the problem is DNS. Check DNS server addresses, VPN settings, and organizational DNS policies. You can also try `ipconfig /flushdns`.

### 10. Check Wi-Fi sign-in
For networks that need a username or password, confirm the user is using the right account and current credentials. Causes include:

- Wrong or recently changed password
- Locked or expired account (see `account-lockout.md`, `password-reset.md`)
- Wrong username or authentication method
- Device not authorized for the network
- Incorrect date and time on the device (this can break certificate checks)

### 11. Check enterprise Wi-Fi sign-in
Corporate Wi-Fi often uses WPA2/WPA3-Enterprise, 802.1X, RADIUS, or certificates. Failures can come from:

- Invalid credentials
- Expired or missing certificate
- Wrong wireless profile
- Device not registered
- Authentication server unavailable
- Incorrect network policy

Problems with authentication infrastructure usually need escalation.

### 12. Forget and reconnect
If the saved profile looks corrupted or holds outdated credentials:

1. Open **Wi-Fi settings** and find the network.
2. Select **Forget**.
3. Reconnect to the correct SSID.
4. Sign in with current approved credentials.
5. Test connectivity.

Command-line alternative:

```cmd
netsh wlan show profiles
netsh wlan delete profile name="ExampleSSID"
```

> **Warning:** Do not remove corporate profiles managed by policy unless authorized.

### 13. Check the wireless driver
An old, corrupted, or incompatible driver can cause drops, missing networks, slow speeds, and adapter crashes.

1. In **Device Manager**, check the adapter status, driver version, driver date, and any error codes.
2. Note any recent driver changes.
3. Identify the exact adapter.
4. Update, reinstall, or roll back to the **approved** driver.
5. Restart if needed and reconnect.

Also check the adapter's **Power Management** tab. If "Allow the computer to turn off this device to save power" is on, it may cause drops after sleep (change only if permitted).

Use only approved drivers.

### 14. Reset the network stack (if authorized)
If earlier steps fail and policy allows, these reset Windows networking components. Restart afterwards.

```cmd
netsh winsock reset
netsh int ip reset
```

> **Note:** These can remove custom network settings. Use only with authorization, and record what you changed.

### 15. Investigate slow Wi-Fi
Check:

- Signal strength and distance from the access point
- Number of devices on the network and congestion
- VPN use, and test with and without VPN where policy allows
- Background downloads or updates
- Device performance (see `slow-computer-troubleshooting.md`)
- Internet service performance

If slowness happens only in one area, investigate the access points serving that area.

### 16. Investigate disconnections
1. Check whether it happens in a specific place.
2. Check signal strength and whether others are affected.
3. Review adapter and driver status.
4. Check whether the device roams between networks or access points.
5. Test on another approved network where appropriate.
6. Check whether it happens after sleep or in power-saving mode.
7. Record how often and when it happens.

To generate a detailed report of recent connection history (run in an elevated window):

```cmd
netsh wlan show wlanreport
```

The report is saved as an HTML file, usually in `C:\ProgramData\Microsoft\Windows\WlanReport`. It shows connection attempts, failures, and reasons.

Frequent drops affecting several users point to a wireless infrastructure problem.

### 17. VPN over Wi-Fi
If Wi-Fi works but the VPN is unstable:

1. Confirm normal internet access and a stable Wi-Fi connection.
2. Test without VPN where appropriate.
3. Check the VPN client status and look for packet loss.
4. Follow `vpn-troubleshooting.md`.

Don't assume Wi-Fi is the cause just because the user is wireless.

### When to escalate
Use `incident-escalation.md` when:

- Several users cannot connect to the same network
- A whole area has lost wireless service, or multiple access points seem down
- DHCP or DNS services appear unavailable
- Enterprise wireless authentication is failing
- Network configuration changes are needed
- Wireless equipment needs physical maintenance
- The problem continues after approved device troubleshooting
- There is evidence of unauthorized wireless activity
- A critical business service is affected

**Include in the escalation:** affected users and device details, network name and location, when it began, IP configuration, test results, error messages, signal information, what you already tried, and the number of devices affected.

### Common Wi-Fi scenarios

| Symptom | Possible cause | What to check |
|---|---|---|
| Network not visible | Adapter, signal, access point, configuration | Adapter, location, available networks |
| Will not connect | Credentials, profile, authentication | SSID, credentials, saved profile |
| Connected but no internet | Gateway, DNS, routing, internet outage | IP settings, gateway, DNS, external ping |
| `169.254.x.x` address | DHCP failure | Renew the IP, check DHCP |
| Keeps disconnecting | Weak signal, driver, interference, infrastructure | Signal, driver, location, other users |
| Slow Wi-Fi | Signal, congestion, bandwidth, device | Signal, network use, device performance |
| Only one user affected | Device configuration or adapter | The user's device |
| Many users affected | Access point or infrastructure | Possible wider incident |
| Corporate Wi-Fi rejects credentials | Account or enterprise Wi-Fi issue | Account status, wireless profile, certificate |
| Wi-Fi works but VPN fails | VPN or authentication issue | `vpn-troubleshooting.md` |
| Internet works but internal resources fail | Internal DNS, routing, or network segmentation | Internal DNS and network access |

---

## Resolution

Apply the fix that matches what you found:

| Finding | Typical resolution |
|---|---|
| Wi-Fi or Airplane mode off | Turn Wi-Fi on and Airplane mode off |
| Wrong network | Connect to the correct SSID |
| Corrupted or outdated saved profile | Forget and reconnect (if permitted) |
| DHCP not giving an address | Renew the IP; escalate if DHCP seems down |
| DNS failure | Check DNS settings; flush the DNS cache; escalate if DNS infrastructure is at fault |
| Driver fault | Update, reinstall, or roll back to the approved driver |
| Weak signal in one location | Move closer; report the dead spot to the network team |
| Account locked or password changed | Follow `account-lockout.md` or `password-reset.md` |
| Certificate or enterprise authentication problem | Escalate |
| Many users affected | Treat as a wider incident and escalate |

### Document the ticket
Record:

- Date and time reported, user, and device
- Wireless network and location
- Symptoms and error messages
- IP configuration findings and connectivity test results
- Troubleshooting performed and changes made
- Resolution and verification results
- Escalation details, if any

> **Never** record passwords, MFA codes, tokens, or other credentials in the ticket.

---

## Verification

- [ ] Symptoms identified and scope determined
- [ ] Wi-Fi on and correct SSID confirmed
- [ ] Signal strength checked
- [ ] Wireless adapter and driver checked
- [ ] IP configuration and DHCP reviewed
- [ ] Default gateway tested
- [ ] Internet connectivity tested
- [ ] DNS resolution tested
- [ ] Wireless sign-in checked
- [ ] Saved profile reviewed where appropriate
- [ ] VPN impact checked where applicable
- [ ] Connection stays stable for a reasonable time
- [ ] Internal resources (drives, apps) reachable where required
- [ ] User confirms they have the connectivity they need
- [ ] Ticket updated, and escalation completed if required

Quick final check:

```cmd
netsh wlan show interfaces
ipconfig
ping 8.8.8.8
nslookup example.com
```

> **In plain English:** the device is on the right network, has a valid address, can reach the internet and company resources, and stays connected.

---

## Prevention

Organizations can reduce Wi-Fi problems by:

- **Maintaining current wireless equipment and good coverage**, and reviewing capacity as usage grows
- **Monitoring access points, DHCP, and DNS** so problems are found before users report them
- **Maintaining enterprise authentication** (RADIUS and certificates), and renewing certificates before they expire
- **Standardizing supported wireless settings and drivers**, and keeping drivers current
- **Pushing managed wireless profiles** so users do not set up corporate Wi-Fi by hand
- **Documenting the wireless network design**, including SSIDs, segmentation, and access point locations
- **Monitoring network performance** and recording problem areas
- **Giving users simple guidance** on connecting, forgetting a network, and when to contact IT

---

## Lessons Learned

- **Find out whether it's one device or many first.** It decides whether you troubleshoot a laptop or declare an incident.
- **Test in layers.** Gateway, then internet by IP, then DNS. Each result narrows the problem.
- **"Connected" does not mean "working."** A user can be on the wrong network or have no internet.
- **Location matters.** Problems that follow a place point to the access point, not the device.
- **A `169.254` address almost always means DHCP failed.**
- **Check the date and time.** A wrong clock can break certificate-based Wi-Fi sign-in.
- **Don't blame Wi-Fi for every VPN problem.** Test without the VPN where allowed.
- **Don't remove managed profiles without permission.** It can lock the user out of corporate Wi-Fi.

### My notes
_Add your own lessons here from lab exercises or real tickets:_

- 

---

## References

- Microsoft Learn: `netsh wlan` commands and the Wireless Network Report (`netsh wlan show wlanreport`)
- Microsoft Learn: `ipconfig`, `ping`, `nslookup`, `Test-NetConnection`, and `Resolve-DnsName`
- Microsoft Learn: Troubleshoot network connectivity in Windows
- Microsoft Learn: 802.1X authentication and enterprise wireless (WPA2/WPA3-Enterprise, RADIUS)
- Your organization's wireless, network, driver, and incident response policies
