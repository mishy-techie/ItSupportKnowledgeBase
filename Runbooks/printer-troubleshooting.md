# Printer Troubleshooting

**Summary:** Printer problems such as "offline" printers, stuck print jobs, failed jobs, and driver errors are among the most common IT support requests. This runbook helps you work out whether the cause is the printer, the network, the user's computer, the print server, or the application, then fix it and confirm it worked.

**Environment:** Controlled IT support environment (Windows computers, network, Wi-Fi, USB, and shared printers, Windows print servers, Windows Print Spooler)

**Related runbooks:** `wi-fi-connectivity.md`, `laptop-setup-checklist.md`, `new-employee-onboarding.md`, `incident-escalation.md`

> **In plain English:** printing is a chain: the application sends a job to Windows, Windows queues it, and the queue passes it over a cable or network to the printer. This runbook checks each link to find the one that is broken.

---

## Symptoms

A user may report:

- Printer is offline or cannot be found
- Documents stay stuck in the print queue
- Print jobs fail, or the printer prints blank or incorrect pages
- Printing works for some people but not others
- Printer keeps going offline
- "Windows cannot connect to the printer"
- Slow printing or duplicate printers
- Works from one computer but not another

---

## Prerequisites

Before starting, make sure you have:

- **Information from the user:** printer name and location, their computer, the application used, how the printer is connected, and whether it ever worked
- **Access:** local administrator rights for spooler and driver tasks (where authorized), and read access to the print server if one is used
- **Tools:** Command Prompt or PowerShell, Printers & scanners settings, Device Manager, and Services
- **Approved drivers:** your organization's approved printer driver source
- **Documentation:** printer list with model, IP address, location, and owner
- **Awareness:** know whether the printer is managed by a print server or installed directly on the computer

---

## Troubleshooting / Implementation

Work through these steps in order.

### 1. Identify the problem
Confirm:

- Printer name, location, model, and connection type (network, Wi-Fi, USB, shared)
- The user's computer, operating system, and application
- Whether it affects one user or many
- Whether it ever worked, and what changed
- Whether other users can print to it

### 2. Check the printer itself
Look at the printer before changing any settings:

- [ ] Powered on, and power cable connected
- [ ] Network cable or Wi-Fi connection active
- [ ] Paper loaded and trays seated correctly
- [ ] Toner or ink available
- [ ] No paper jams, and covers and doors closed
- [ ] No error on the display

Record any error code shown.

### 3. Decide whether it is local or widespread
Ask whether others can print to the same printer.

| Result | Likely area |
|---|---|
| **Others can print** | The user's computer, driver, queue, permissions, or application |
| **Many people cannot print** | Printer hardware, network, print server, IP address, or print service |

### 4. Check network connectivity (network and Wi-Fi printers)
Find the printer's IP address from its display or configuration page, DHCP records, the print server, or your network management tool. Then test from an authorized workstation:

```cmd
ping <printer-ip-address>
```

```powershell
Test-NetConnection -ComputerName <printer-ip-address> -Port 9100
```

Port 9100 is commonly used for direct network printing. Some printers use other ports (such as 631 or 515), so check the printer documentation.

> **Notes:**
> - A successful ping shows the device responds, not that printing works.
> - A failed ping alone does not prove it is unreachable, because some printers or networks block ping.

### 5. Verify the IP address
If a printer's IP address changed, computers still pointing at the old address will show it as offline or leave jobs stuck.

1. Compare the printer's current IP with the address set on the computer or print server.
2. Check the printer port settings:

   ```powershell
   Get-Printer | Select-Object Name, PortName, DriverName, PrinterStatus
   Get-PrinterPort | Select-Object Name, PrinterHostAddress
   ```

3. Update the port if the address changed.

Printers should use a **DHCP reservation or static address** so the address does not change.

### 6. Check the printer status in Windows
In **Settings > Bluetooth & devices > Printers & scanners** (or Control Panel), confirm:

- [ ] The printer is present and correctly selected
- [ ] It is not paused
- [ ] **Use Printer Offline** is not ticked
- [ ] The right printer is set as default where appropriate

Remove unnecessary duplicate entries that confuse users.

> **Tip:** A printer that keeps going "offline" even when the network is fine is sometimes caused by its port type (for example, WSD ports) or by SNMP status checking. Switching to a standard TCP/IP port with the correct address is a common fix. Follow your organization's approved process.

### 7. Check the print queue
List jobs:

```powershell
Get-PrintJob -PrinterName "ExamplePrinter"
```

Look for jobs stuck in "Printing", jobs with errors, duplicates, or a large file blocking others. If appropriate:

1. Cancel the affected job.
2. Clear unnecessary pending jobs.
3. Send a new test print.

```powershell
Remove-PrintJob -PrinterName "ExamplePrinter" -ID 1
```

If jobs will not delete, go to the next step.

### 8. Restart the Print Spooler
The Print Spooler service manages print jobs. In an elevated window:

```powershell
Get-Service -Name Spooler
Restart-Service -Name Spooler
Get-Service -Name Spooler
```

The service should report **Running**. Then reopen the queue, check whether stuck jobs cleared, and send a test print.

### 9. Clear a stuck queue
If the queue is still stuck, an authorized technician can clear the spool files:

1. Stop the Print Spooler.
2. Delete the pending files in the spool folder.
3. Start the Print Spooler.
4. Reopen the queue and send a test print.

```powershell
Stop-Service -Name Spooler
Remove-Item -Path "C:\Windows\System32\spool\PRINTERS\*" -Force
Start-Service -Name Spooler
```

> **Warning:** Only delete spool files while the service is stopped, and follow your organization's procedures. This removes **all** waiting jobs for every user on that computer or server, so warn people first.

### 10. Check the printer driver
A wrong, corrupted, or outdated driver can cause failed jobs, blank pages, bad formatting, application crashes, and missing features.

```powershell
Get-PrinterDriver | Select-Object Name, Manufacturer, DriverVersion
```

Before changing a driver:

- Confirm the printer model and operating system
- Use an **approved driver source** and check compatibility
- Follow your organization's software installation process

Avoid drivers from untrusted third-party websites.

### 11. Reinstall or update the driver
1. Record the current printer configuration.
2. Remove the printer if appropriate.
3. Remove the problem driver if required.
4. Install the approved driver.
5. Re-add the printer and check its settings.
6. Send a test print.

If a print server manages the printer, use the print server process instead of installing drivers on individual computers.

### 12. USB printers
1. Confirm the printer is on and the cable is secure.
2. Try another approved USB port.
3. Check **Device Manager** to see whether the printer is detected.
4. Check the driver.
5. Remove and reconnect the printer if appropriate.
6. Send a test print.

Don't keep unplugging and replugging without working out whether the cause is physical, driver, or configuration.

### 13. Wi-Fi printers
Check:

- The printer is on the expected Wi-Fi network (not a guest network)
- The signal is adequate and the printer has a valid IP address
- The computer and printer are allowed to communicate (network segmentation)
- The printer has not switched networks, and wireless credentials have not changed

See `wi-fi-connectivity.md` for general Wi-Fi checks.

### 14. Print servers
If a print server manages the printer, work out whether the problem affects one computer, many computers, one printer, or many printers. Check:

- Printer status and queue
- Driver availability
- Spooler service
- Printer port configuration
- Recent configuration changes

Do not make unauthorized changes to production print servers. If several printers fail at once, escalate to the infrastructure team.

### 15. Application-specific problems
If other applications print fine but one does not:

1. Confirm the correct printer is selected.
2. Test printing from another application.
3. Check the application's print settings, page size, and orientation.
4. Use print preview.
5. Restart the application and test again.

If other applications work, the printer and Windows printing are probably fine and the problem is in that application.

### 16. Run a controlled test print
After each change, test with:

- A Windows test page (where appropriate)
- A simple text document
- A PDF
- A document from the affected application

Confirm the correct printer, correct output and formatting, no errors, and a queue that clears. Use simple test documents instead of confidential ones.

### When to escalate
Use `incident-escalation.md` when:

- Printer hardware looks faulty, or a replacement is needed
- Several printers are affected
- Network connectivity cannot be restored
- The print server is unavailable
- Network configuration changes are needed
- The printer needs vendor maintenance
- The problem continues after approved troubleshooting
- A critical business process is affected
- Changes need access outside your authority

**Include in the escalation:** printer name, location, model, and IP address; symptoms and error codes; test results; troubleshooting performed; when it started; and the number of users affected.

### Common printer scenarios

| Symptom | Possible cause | What to check |
|---|---|---|
| Printer shows offline | Network, IP, driver, port type, or printer state | Physical state, connectivity, IP, Windows status |
| Job stuck | Print Spooler or corrupt job | Clear the queue and restart the spooler |
| One user cannot print | Computer or user configuration | Local printer settings and driver |
| Many users cannot print | Printer, network, or print server | Shared infrastructure |
| Suddenly stopped working | IP address or network change | Printer IP and connectivity |
| Blank pages | Driver, toner or ink, or hardware | Consumables and driver |
| Wrong formatting | Driver or application settings | Test another application and verify the driver |
| USB printer not detected | Cable, port, driver, or hardware | USB connection and Device Manager |
| Wi-Fi printer unavailable | Wireless or network segmentation | Network connection and accessibility |
| Jobs return errors | Driver, spooler, permissions, or printer | Queue, driver, and event information |
| Duplicate printers | Printer added more than once | Remove duplicates, keep the approved one |

---

## Resolution

Apply the fix that matches the finding:

| Finding | Typical resolution |
|---|---|
| Printer off, jammed, or out of paper or toner | Fix the physical problem |
| Printer IP address changed | Update the port; reserve the address |
| Printer paused or set offline | Clear the setting in Windows |
| Stuck jobs | Cancel jobs, restart the spooler, or clear the spool folder |
| Faulty driver | Install the approved driver |
| USB fault | Change the cable or port, reinstall the printer |
| Wi-Fi or segmentation issue | Fix the network connection or escalate to the network team |
| Print server problem | Follow print-server procedures or escalate |
| Application-specific | Fix the application settings or escalate to application support |
| Hardware failure | Escalate for repair or replacement |

### Document the ticket
Record:

- Date and time reported, affected users, printer name, location, and model
- Symptoms, error messages, and connectivity status
- Troubleshooting performed and changes made
- Test results and the resolution
- Escalation details, if any

> **Never** record passwords, tokens, or other credentials in the ticket.

---

## Verification

- [ ] Printer powered on and showing no unresolved errors
- [ ] Paper and consumables checked
- [ ] Printer connectivity verified
- [ ] Printer IP address verified where applicable
- [ ] Printer status checked (not paused or offline)
- [ ] Print queue checked and clear
- [ ] Print Spooler running
- [ ] Printer driver checked
- [ ] Test print completed successfully
- [ ] Application-specific printing tested where required
- [ ] Printer stays online after a short wait
- [ ] User confirms successful printing
- [ ] Ticket updated, and escalation completed if required

Quick final check:

```powershell
Get-Service -Name Spooler
Get-Printer -Name "ExamplePrinter" | Select-Object Name, PrinterStatus
Get-PrintJob -PrinterName "ExamplePrinter"
```

> **In plain English:** the printer is on and reachable, the queue is empty, a test page printed correctly, and the user can print what they need.

---

## Prevention

Organizations can reduce printer problems by:

- **Using DHCP reservations or static IP addresses** for managed printers
- **Keeping approved drivers current** and deploying them from a central source
- **Standardizing supported printer models**
- **Monitoring printer availability and consumables** so toner and paper are replaced before they run out
- **Maintaining printer firmware** on a schedule
- **Documenting printers** (location, owner, model, IP address, and print server)
- **Documenting the print server and network setup**
- **Removing obsolete printers and drivers** from computers and servers
- **Giving users simple guidance** on selecting the right printer and reporting problems

> **Security note:** The Print Spooler service has had serious security vulnerabilities. Keep print servers and computers patched, and follow your organization's guidance on where the spooler should be enabled. For example, it is commonly disabled on domain controllers.

---

## Lessons Learned

- **Look at the printer first.** Paper jams, empty toner, and loose cables cause many "IT" problems.
- **One user or many?** This single question decides whether to check a computer or the infrastructure.
- **Offline does not always mean off.** Often the printer's IP address changed or the port is wrong.
- **Restart the spooler before anything drastic.** It is quick and safe.
- **Clearing the spool folder affects everyone's jobs.** Warn users first.
- **Use approved drivers.** Random downloads cause new problems.
- **Test with a simple page.** It avoids printing confidential documents and isolates the issue.
- **Fix the cause, not just the symptom.** A printer that keeps going offline needs a stable IP or port change.

### My notes
_Add your own lessons here from lab exercises or real tickets:_

- 

---

## References

- Microsoft Learn: Windows printing and the PrintManagement PowerShell module (`Get-Printer`, `Get-PrintJob`, `Get-PrinterDriver`, `Get-PrinterPort`)
- Microsoft Learn: Print Spooler service and security guidance
- Microsoft Learn: `Test-NetConnection` and `ping`
- Printer manufacturer documentation (models, drivers, ports, and error codes)
- Your organization's approved driver, print server, and incident response procedures
