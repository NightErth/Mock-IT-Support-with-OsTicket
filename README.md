# Mock IT Support Desk with osTicket

A self-hosted IT ticketing system built with osTicket and Docker, configured and run like a real Tier 1/2 help desk; from raw, unstructured tickets to full ITIL-style triage, SLA tracking, a self-service knowledge base, and automated routing rules.

---

## Overview

This project is a hands-on lab where I stood up my own help desk from scratch using osTicket (an open-source ticketing platform) running in Docker containers, then processed a simulated flood of 12 support tickets the way an actual IT support agent would.

The goal wasn't just to install software and call it done. I wanted to go through the full lifecycle: watch what a ticketing system looks like with zero configuration, feel why that's a problem at scale, then build out departments, SLA plans, help topics, and automation rules to fix it. Finally, I triaged and resolved every ticket, wrote knowledge base articles, and set up a filter to auto-route tickets without any manual sorting.

---

## Project Objectives

The lab simulates a small IT department receiving a mixed batch of common employee requests such as locked accounts, VPN failures, onboarding requests, printer issues, and so on, with no existing structure in place.

The learning goals were to:

- Set up a ticketing platform in a containerized environment using Docker
- Understand what happens when tickets have no priority, SLA, or department assigned
- Build out an ITIL-style structure (departments, help topics, SLA plans) to organize incoming requests
- Triage and resolve tickets in priority order rather than first-come-first-served
- Write public knowledge base articles to reduce repeat tickets
- Configure a filter rule to automatically route and prioritize tickets based on content

---

## Lab Environment

| Component | Details |
|---|---|
| Ticketing Platform | osTicket (open-source) |
| Deployment | Docker Desktop + Docker Compose (2 containers: osTicket app + MySQL database) |
| Operating System | Windows |
| Interfaces | Client Portal (end-user) and Staff Control Panel (agent/admin) |
| Departments | 3 (including Network and Infrastructure) |
| Help Topics | 6 (e.g. Password Reset, VPN Issue) |
| SLA Plans | 4 total — Default SLA (18 hrs), Standard (24 hrs), High (4 hrs), Critical (1 hr) |
| Knowledge Base | Public FAQ category with 3 published articles |
| Automation | 1 filter rule for VPN-related ticket auto-routing |
| Tickets Processed | 12 simulated employee support requests |

---

## Project Workflow

**1. Environment setup**
Installed Docker Desktop on Windows and wrote a Docker Compose file to spin up two containers; one running osTicket, one running MySQL and connected them together. Confirmed everything was working by loading the Staff Control Panel in the browser.

![osTicket installed and running](https://github.com/NightErth/Mock-IT-Support-with-OsTicket/blob/main/01%20OsTicket%20Installed.png)

**2. Submitting the first ticket**
Before touching any settings, I submitted one ticket through the Client Portal exactly as an end user would, then checked it from the agent side in the Staff Control Panel.

![First ticket submitted with no priority or SLA](https://github.com/NightErth/Mock-IT-Support-with-OsTicket/blob/main/02%20First%20Ticket.png)

With no configuration in place yet, the ticket landed with no department, no SLA due date, and a generic default priority. There was nothing in the system telling an agent this needed urgent attention versus routine follow-up.

**3. Flooding the queue**
Submitted 11 more tickets through the portal, each representing a different real-world issue, i.e. VPN failures, onboarding requests, printer problems, account lockouts, and similar common complaints. With 12 tickets sitting side by side, all defaulting to the same department and priority, it became obvious that an agent would have to manually re-read every single ticket just to figure out what was actually urgent.

![Queue of 12 tickets with no priority assigned](https://github.com/NightErth/Mock-IT-Support-with-OsTicket/blob/main/03%20All%20Tickets%20Without%20%20any%20priority.png)

**4. Building the ITIL structure**
Configured three departments, four SLA plans with different grace periods, and six help topics, each help topic pre-linked to the correct department and SLA plan. This meant that going forward, selecting a help topic at submission time would automatically route the ticket and assign it a due date, instead of relying on an agent to guess.

![SLA plans with grace periods configured](https://github.com/NightErth/Mock-IT-Support-with-OsTicket/blob/main/04%20SLA%20.png)

Grace periods were set based on urgency: Critical tickets get a 1-hour window, High gets 4 hours, and Standard/Default sit at 24 and 18 hours respectively. A 1-hour SLA on an account lockout means "respond now." A 24-hour SLA on a software install request means "this matters, but it can wait its turn."

**5. Triage and resolution**
Went back through all 12 tickets and reassigned each one to the correct help topic, which automatically applied the right department and SLA. Then worked the queue in priority order with most urgent first and adding internal notes for tracking and public replies for the end user, and closed each ticket out.

![All tickets resolved](https://github.com/NightErth/Mock-IT-Support-with-OsTicket/blob/main/05%20All%20Tickets%20Resolved.png)

**6. Knowledge base**
Wrote three public FAQ articles covering the most common issues in the batch (password resets, VPN troubleshooting, Wi-Fi reconnection after an OS update) so future users have somewhere to look before opening a ticket.

![Knowledge base FAQ articles](https://github.com/NightErth/Mock-IT-Support-with-OsTicket/blob/main/06%20Knowledge%20Base%20FAQ.png)

**7. Automation**
Built a filter rule that watches for VPN-related tickets at the moment they're created and automatically sets the department to Network and Infrastructure, the SLA to High, and the priority to High, before any agent has looked at it.

![VPN ticket auto-routed by filter rule](https://github.com/NightErth/Mock-IT-Support-with-OsTicket/blob/main/07%20Automate%20Ticket%20Routing%20with%20Filtering.png)

---

## Ticket Categories

Based on the 12 tickets processed, the following categories came up:

- Password reset / account lockout
- VPN connectivity issues
- New employee / researcher onboarding
- Software and license installation requests (e.g. Microsoft 365, Python admin rights)
- Printer issues
- Wi-Fi connectivity after OS update
- MFA / multi-factor authentication problems
- Email and calendar sync issues (Outlook)
- Hardware access issues (USB drives, shared drives)
- Zoom / meeting room equipment issues

---

## Priority & SLA Management

Tickets were assigned one of four priority tiers, each tied to a matching SLA plan:

| Priority / SLA | Grace Period | Example Use Case |
|---|---|---|
| Critical | 1 hour | Account lockouts, security issues |
| High | 4 hours | VPN outages, connectivity issues blocking work |
| Standard | 24 hours | Software installs, non-urgent access requests |
| Default SLA | 18 hours | Fallback for uncategorized tickets |

Priority and SLA were no longer something an agent had to eyeball, they were determined automatically by which help topic the ticket was filed under. This meant the queue itself told you what to work on next instead of requiring someone to read all 12 tickets and guess. Resolution tracking came from working strictly in SLA order: shortest grace period first, so nothing with a tight deadline sat waiting behind lower-priority requests.

---

## Knowledge Base

The knowledge base exists to cut down on repeat tickets for problems that have a known, simple fix. I published three FAQ articles under a "Common IT Issues" category:

- **How to Reset Your Password**
- **VPN Connection Troubleshooting Guide**
- **Connecting to Wi-Fi After an OS Update**

These were picked because they matched the most common issue types in the 12-ticket batch. The idea is straightforward: if a user can self-serve a password reset from an FAQ page, that's one less ticket sitting in the queue competing for an agent's attention.

---

## Automation

One filter rule was built and tested: a VPN auto-routing filter.

**What it does:**
- Triggers automatically when a new ticket comes in with "VPN" in the subject
- Sets the department to **Network and Infrastructure**
- Sets the SLA plan to **High**
- Sets the priority to **High**

**Why it matters:**
This runs the moment the ticket is created before any agent opens it. The triage decision ("VPN issue = Network and Infrastructure, High priority, 4-hour SLA") is encoded into the system ahead of time instead of being made ticket by ticket. I tested this by submitting a new VPN ticket and confirming it landed already routed and prioritized with zero manual intervention, which is visible in the ticket's audit trail (see the "VPN Auto-Route" log entries on the ticket).

---

## Skills Demonstrated

- IT ticket management and queue triage
- Incident handling and prioritization (P1–P4 equivalent)
- ITIL-aligned workflow (categorization, SLA management, resolution documentation)
- SLA configuration and grace period management
- Help desk operations from both end-user and agent perspective
- Knowledge base creation and administration
- Filter/automation rule configuration for ticket routing
- Docker and Docker Compose for containerized application deployment
- Basic system administration (departments, user roles, help topics)
- Technical documentation and internal/external communication on tickets

---

## Key Takeaways

Running through this lab made the value of SLA and priority structure obvious in a way that reading about ITIL never quite does. With one ticket sitting in the queue, the lack of structure doesn't feel like a problem. With twelve tickets sitting there identically formatted, it's immediately clear that an agent has no way to tell a locked-out executive apart from a jammed printer without reading every single one.

Building the departments, SLA plans, and help topics first, then going back to retriage, showed how much of "good IT support" is actually front-loaded configuration work rather than something you figure out ticket by ticket. The automation piece was the clearest payoff, once the VPN filter was in place, a ticket that used to need a human to read, categorize, and route was handled correctly with zero clicks.

---

## Future Improvements

- **Active Directory integration** — sync user accounts instead of manually creating them in osTicket
- **Email ticket ingestion** — allow tickets to be created directly from incoming support emails instead of only the web portal
- **LDAP authentication** — centralize agent login instead of managing local osTicket credentials
- **Asset management** — tie tickets to specific devices/hardware for better tracking
- **Reporting dashboards** — build out SLA compliance and ticket volume reporting over time
- **Additional filter rules** — expand automation beyond VPN tickets to other high-volume categories like password resets

