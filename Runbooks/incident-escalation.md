# Incident Escalation

**Summary:** Incident escalation is handing an IT problem to the right team when it goes beyond your access, skills, or authority. This runbook explains how to decide when to escalate, who to send it to, what to include, and how to follow it through to resolution.

**Environment:** Controlled IT support environment (Service Desk or IT Support team with a ticketing system, support tiers, and service-level agreements)

**Related runbooks:** `account-lockout.md`, `blue-screen-of-death-bsod.md`, `vpn-troubleshooting.md`, `wi-fi-connectivity.md`, `employee-offboarding.md`

> **In plain English:** escalating is not failing. It is passing the problem to someone better placed to fix it, with a clear note so they don't have to start from scratch.

---

## Prerequisites

Before escalating, make sure you have:

- **Access to the ticketing system** and the organization's priority matrix
- **The escalation contacts:** support tiers, on-call details, security team, vendors, and the major incident process
- **Awareness of SLAs:** response and resolution targets for the incident's priority
- **The relevant troubleshooting runbook** for the issue, already worked through
- **Authority limits:** know what you are and are not permitted to change

---

## Troubleshooting / Implementation

Follow these steps in order. If you spot a security issue or a possible major incident at any point, jump straight to the matching step.

### 1. Understand the incident
Find out and record, without assuming the cause:

- What is not working, and who is affected?
- When did it start, and is it still happening?
- What changed beforehand?
- Can it be reproduced?
- What is the business impact, and is there a workaround?

### 2. Set the priority
Priority comes from **business impact and urgency**, not how technically complex the issue is. Use your organization's matrix. A typical model:

| Priority | Typical impact | Example |
|---|---|---|
| **P1 Critical** | Organization-wide or critical service outage | Core business service unavailable |
| **P2 High** | Major business impact | A department cannot do critical work |
| **P3 Medium** | Limited business impact | One user cannot reach a required application |
| **P4 Low** | Minimal impact | Non-critical issue with a workaround |

Factors to weigh: number of users and devices affected, how critical the service is, security impact, revenue impact, location, and whether a workaround exists.

### 3. Do appropriate first-line troubleshooting
Before escalating, do what is **safe, authorized, relevant, and documented**:

- Follow the relevant runbook
- Check for known outages or recent changes
- Check account status and basic connectivity
- Test another device or network
- Review basic logs
- Apply approved fixes

Do not make high-risk changes just to avoid escalating.

### 4. Decide whether to escalate
Escalate when:

- The fix needs permissions you don't have
- Specialist knowledge is needed
- Infrastructure is involved
- Multiple users are affected
- A critical business service is affected
- A security concern exists
- Standard troubleshooting is exhausted and no workaround exists
- The change is outside the Service Desk's authority
- The SLA is close to being breached, or already has been
- Vendor support is needed

### 5. Choose the right team

| Team | Typically handles |
|---|---|
| **Service Desk / Level 1** | Password resets, account lockouts, basic workstation, application, and connectivity issues, standard software installs |
| **Desktop Support / Level 2** | Complex workstation problems, hardware, drivers, OS issues, repeated BSODs |
| **Network team** | Outages, routing, switches, wireless infrastructure, DNS/DHCP, VPN infrastructure |
| **Systems / Infrastructure** | Servers, Active Directory and identity infrastructure, storage, virtualization |
| **Security team** | Malware, account compromise, phishing, unauthorized access, data security incidents |
| **Application support** | Business application failures, configuration, and integrations |
| **Vendor support** | Product defects, hardware warranty, unsupported behavior |

### 6. Security escalation (act immediately)
Escalate **right away** through your security incident process if you see or suspect:

- Malware or ransomware
- Account compromise or credential theft
- Unauthorized access or data exfiltration
- Phishing
- Suspicious sign-ins or privileged activity
- Lost or stolen company devices
- Unauthorized software

> **Do not** delete suspicious files, reset or wipe devices, or keep troubleshooting if it could destroy evidence. Wait for direction from the security team.

### 7. Major incident escalation
Treat it as a possible major incident when:

- Several departments or many users are affected
- A critical service, core network, authentication, or organization-wide email is down
- A major cloud service is unavailable
- There is no effective workaround
- A security incident has significant business impact

Follow your organization's major incident process. For P1 and major incidents, contact the right people directly (phone or chat as per policy) as well as updating the ticket.

### 8. Vendor escalation
Before contacting a vendor, gather:

- Product name, version, and support or license details
- Error messages and timestamps
- Affected users and systems
- Relevant logs and configuration details
- Troubleshooting performed and steps to reproduce

Never share credentials or confidential information with a vendor unless authorized and handled through approved procedures.

### 9. Functional escalation (approval needed)
Sometimes the problem is understood but you need approval or someone else to make the change, for example:

- Privileged access requests or permission changes
- Security policy exceptions
- Firewall, network, or production system changes
- Data recovery
- Software licensing exceptions
- Device replacement

Get the right authorization **before** making the change.

### 10. Document before you hand over
Update the ticket with:

- Incident summary, priority, and current status
- User and business impact
- Date, time, and symptoms
- Exact error messages
- Affected users, systems, and devices
- Recent changes
- Troubleshooting performed **and the results**
- Relevant logs and sanitized screenshots
- Workaround attempted
- Reason for escalation and destination team

> **Never** put passwords, MFA codes, tokens, or other secrets in the ticket.

### 11. Hand over clearly
1. Select the correct team.
2. Confirm or update the priority.
3. Provide the technical summary.
4. Record what has been tried.
5. Note the suspected area of failure, if known.
6. Attach evidence.
7. State exactly what you need from them.
8. Get acknowledgment from the receiving team where required.

Avoid vague notes such as "Please investigate."

**Escalation note template** (use your organization's format where one exists):

```text
Incident:
[Brief description]

Impact:
[Who or what is affected]

Priority:
[P1 / P2 / P3 / P4]

Started:
[Date and time]

Symptoms:
[What was observed]

Error:
[Exact error message or code]

Affected Systems:
[Systems, devices, or services]

Troubleshooting Completed:
[Actions taken and the result of each]

Recent Changes:
[Known changes]

Workaround:
[Available workaround, or "None"]

Reason for Escalation:
[Why this is beyond current support scope]

Requested Action:
[What the receiving team needs to do]
```

**Example (fictional):**

```text
Incident:
Finance team cannot open the shared accounting application.

Impact:
12 users in Finance; month-end close at risk.

Priority:
P2

Started:
2026-01-15 09:10

Symptoms:
Application shows "Unable to connect to server" on all affected PCs.

Error:
Connection timed out (error code 10060)

Affected Systems:
Accounting application (APP-SRV01), Finance floor PCs

Troubleshooting Completed:
Tested from two PCs and another network: same result. Other apps work normally.
Server pings but the application port does not respond.

Recent Changes:
Server patching was scheduled overnight.

Workaround:
None

Reason for Escalation:
Likely server-side issue; no access to the application server.

Requested Action:
Check application service status on APP-SRV01 and review last night's patching.
```

### 12. Keep the user informed
Tell the user:

- That the incident has been escalated, and why
- Which team has it
- The next steps and any workaround
- Whether you need more information from them

Do not give unsupported technical conclusions, and do not promise a fix time unless an approved SLA or commitment exists.

### 13. Watch the SLA
Track response and resolution targets, priority, the current owner, pending actions, and user updates. **Escalate internally before the SLA is breached**, not after.

### 14. Follow up after escalation
- Monitor the ticket and answer requests for information
- Update the user and record major changes
- Track any workaround
- Avoid passing ownership around without a clear reason
- Confirm the resolution when the work is done

### Common escalation scenarios

| Scenario | Send to | Include |
|---|---|---|
| Repeated BSOD | Desktop Support | Stop code, dump information, recent changes, troubleshooting |
| Organization-wide Wi-Fi outage | Network team | Locations, timestamps, connection symptoms |
| VPN down for multiple users | Network / Infrastructure | Affected users, error messages, VPN status |
| Suspected malware | Security team | Symptoms, device, observed activity |
| Account compromise | Security / Identity team | Account, suspicious activity, sign-in details |
| Business application failure | Application support | Application, error, affected users, how to reproduce |
| Server unavailable | Infrastructure team | Server or service, symptoms, timestamps, impact |
| Hardware failure | Desktop Support / Vendor | Device details, diagnostics, symptoms |
| Missing company laptop | Security / Asset management | Asset details, last known status, user report |
| SLA approaching | Appropriate escalation team | Priority, status, outstanding actions |

---

## Resolution

An escalated incident is resolved when:

1. The receiving team has fixed the problem, or an approved workaround is in place.
2. The affected service or device has been tested.
3. The user confirms they can do what they need to do.
4. No related errors remain.
5. The final ticket is complete, with:
   - The original issue, impact, and priority
   - A timeline and the troubleshooting done
   - Escalation details
   - Root cause (if known) and the resolution
   - Any workaround and the user's confirmation
   - Preventive actions identified

For major incidents, follow your organization's formal validation and closure process. Do not close a ticket with thin documentation.

---

## Verification

### Before you escalate
- [ ] Incident summary is clear
- [ ] User and business impact documented
- [ ] Priority assigned
- [ ] Affected systems and users identified
- [ ] Error messages and recent changes recorded
- [ ] Troubleshooting completed and results documented
- [ ] Logs and sanitized screenshots attached where useful
- [ ] Workarounds documented
- [ ] Correct team selected and reason for escalation recorded
- [ ] Required authorization identified
- [ ] Receiving team acknowledged the handover (where required)
- [ ] User informed
- [ ] SLA status reviewed

### Before you close
- [ ] Fix or approved workaround confirmed
- [ ] Service or device tested
- [ ] User confirms the impact is resolved
- [ ] Ticket documentation completed
- [ ] Follow-up actions assigned
- [ ] Knowledge base or problem management update considered
- [ ] Post-incident review completed where required

> **In plain English:** the right team has the full story, the user knows what is happening, and the problem is fixed, tested, and written up.

---

## Prevention

Escalation can't be eliminated, but unnecessary or poor escalations can be reduced.

### Reduce avoidable escalations
- **Keep runbooks up to date** so first-line staff can fix more
- **Train staff** on common issues and on what is within their authority
- **Share known issues and workarounds** in the knowledge base
- **Use approved self-service tools** for common requests

### Escalate better
- **Use a standard escalation template** so nothing is missing
- **Maintain a current contact and on-call list** for every team
- **Agree clear priority definitions** and SLAs
- **Escalate early** when an SLA is at risk

### Learn from incidents
- **Update the knowledge base and runbooks** after recurring incidents
- **Raise a problem management investigation** for repeating issues
- **Hold post-incident reviews** for major or recurring incidents

For a post-incident review, ask:

- What happened and what was the impact?
- When was it detected, and how quickly was it escalated?
- Was the right team engaged?
- What caused it, and what fixed it?
- Was communication effective, and was the SLA met?
- What would prevent it happening again?

---

## Lessons Learned

- **Escalate on impact, not on difficulty.** A simple problem affecting a critical service can outrank a complex one affecting one person.
- **A good handover note saves hours.** "Please investigate" forces the next team to start again.
- **Record results, not just actions.** "Restarted the service" is less useful than "Restarted the service, still failing with error 10060."
- **Escalate early.** Waiting until an SLA is breached removes your options.
- **Security issues change the rules.** Preserve evidence and let the security team lead.
- **Keep the user informed.** Honest updates matter more than optimistic time estimates.
- **Don't bounce tickets.** Repeated reassignment delays resolution.
- **Feed lessons back.** Every escalation is a chance to improve a runbook.

### My notes
_Add your own lessons here from lab exercises or real tickets:_

- 

---

## References

- ITIL framework guidance on incident management, escalation, and problem management
- NIST SP 800-61, Computer Security Incident Handling Guide
- Your organization's priority matrix, SLAs, and major incident process
- Your organization's security incident response and data handling policies
- Vendor support documentation for the products in your environment
