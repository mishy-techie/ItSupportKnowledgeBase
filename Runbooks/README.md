# Runbooks

Step-by-step guides for handling common IT support tasks and problems. A runbook is a checklist someone can follow to fix an issue or complete a routine task the same way every time, even if they haven't done it before.

**Who is this for?**
- **Non-technical readers:** See how common IT problems are handled in a structured, repeatable way.
- **Technical readers:** Find diagnostic steps, resolutions, and escalation paths you can reuse or adapt.

---

## 🧩 Runbook Structure

Each runbook follows the standard Knowledge Base template where it applies:

| Section | Purpose |
|---|---|
| **Prerequisites** | Tools, access, and information needed before starting |
| **Troubleshooting / Implementation** | Diagnostic or setup steps, in order |
| **Resolution** | The fix |
| **Verification** | How to confirm the problem is actually solved |
| **Prevention** | How to avoid a repeat |
| **Lessons Learned** | Notes from real use |
| **References** | Official documentation and sources |

Not every runbook needs every section. The structure is adapted to the procedure. For example, an onboarding checklist may not need a Troubleshooting section.

---

## 🚨 Escalation Philosophy

Not every issue should be resolved at the first level of support.

**Consider escalating when:**
- The issue needs administrative privileges you don't have
- Multiple users or systems are affected
- A security incident is suspected
- The problem involves infrastructure outside the support team's responsibility
- Troubleshooting steps have been exhausted
- Specialist knowledge is required
- Continuing could cause data loss or service disruption
- A business-critical service is affected

**When escalating, include:**
- What the issue is and who or what is affected
- When it started and how often it happens
- Steps already tried and their results
- Error messages or screenshots (sanitized)
- Business impact and urgency

This lets the receiving team continue the investigation without repeating completed work.

---

## 🔒 Security & Privacy

These runbooks are intended for controlled IT support environments.

**Never include:**
- Passwords, MFA codes, or authentication tokens
- Private keys or API credentials
- Production secrets
- Sensitive personal information
- Confidential company information
- Real usernames, hostnames, or IP addresses

Screenshots and examples must use sanitized or fictional information.

---

## 📈 Continuous Improvement

Runbooks evolve. Update the relevant runbook when a procedure reveals:

- A new root cause
- A more efficient troubleshooting step
- A recurring issue
- A missing diagnostic check
- A better resolution
- A new escalation path

The goal is a practical knowledge base that becomes more useful with every issue handled.

---

## 📂 Current Runbook Collection

```text
Runbooks/
│
├── README.md
│
├── password-reset.md
├── account-lockout.md
│
├── printer-troubleshooting.md
├── vpn-troubleshooting.md
├── wi-fi-connectivity.md
├── slow-computer-troubleshooting.md
├── blue-screen-of-death-bsod.md
│
├── laptop-setup-checklist.md
├── new-employee-onboarding.md
├── employee-offboarding.md
│
└── incident-escalation.md
```

| Category | Runbook | Covers |
|---|---|---|
| **Accounts** | `password-reset.md` | Resetting a user's password |
| | `account-lockout.md` | Unlocking accounts after failed sign-ins |
| **Troubleshooting** | `printer-troubleshooting.md` | Printing problems |
| | `vpn-troubleshooting.md` | Remote connection problems |
| | `wi-fi-connectivity.md` | Wireless network issues |
| | `slow-computer-troubleshooting.md` | Performance problems |
| | `blue-screen-of-death-bsod.md` | Windows crash errors |
| **Setup & Lifecycle** | `laptop-setup-checklist.md` | Preparing a new laptop |
| | `new-employee-onboarding.md` | Setting up a new starter |
| | `employee-offboarding.md` | Removing access for leavers |
| **Process** | `incident-escalation.md` | When and how to escalate |

---

## 📌 Status

**Active and under development.** More runbooks will be added as the IT Support Knowledge Base grows.
