# Auto Ticket Classification using Flow Designer

ServiceNow group project — **ServiceNow System Administrator (NM-ENG)** track on SkillWallet.

A school IT helpdesk receives incident requests daily (Wi-Fi issues, projector failures, password problems, slow computers). Staff previously triaged every ticket manually. This project automates ticket classification with **ServiceNow Flow Designer**: when a ticket is created in the custom **School IT Ticket** table with an empty Category, the flow reads keywords in the *Short description* and sets Category + Subcategory, then emails the caller a confirmation.

## Flow: "Auto Classify School IT Tickets" (Global — Published/Active)

Trigger: **Record Created** on `School IT Ticket` where **Category is empty**

| Condition (Short description contains) | Category set | Subcategory set |
|---|---|---|
| WiFi / Wi-Fi / Network | Network | Wi-Fi |
| Projector | Hardware | Projector |
| Forgot password | Access | Forgot Password |
| Slow Computer | Performance | Slow Computer |

After classification the flow sends an email to the Caller:
- **Subject:** Your Request for the issue has been submitted.
- **Body:** Ticket confirmation message.

## Backend configuration (Milestone 2)

Custom table **School IT Ticket** (`u_school_it_ticket`) with auto-numbering (SCH prefix) and fields:
Number (auto), Caller (ref sys_user), Category (choice), Subcategory (choice, **dependent on Category**), Short description (string), Description (string), State (choice), Assigned Group (ref sys_user_group), Assigned to (ref sys_user).

Category → Subcategory dependency: Wi-Fi→Network, Projector→Hardware, Forgot Password→Access, Slow Computer→Performance.

## Testing (Milestone 4)

| Ticket | Short description | Auto-assigned | Result |
|---|---|---|---|
| SCH0001004 | WiFi not working in library | Network / Wi-Fi | ✅ |
| SCH0001005 | Projector not turning on. | Hardware / Projector | ✅ |

Confirmation emails logged in System Logs → Emails with subject "Your Request for the issue has been submitted." (4/4 tickets).

## Deployment (Milestone 5)

Update set **"Project Update Set"** (Global) set to **Complete** — 35 customer updates captured (table, dictionary, choice lists, flow). Exported to XML for sharing.

## Screenshots

1. `1-flow-designer-auto-classify-published.png` — Flow Designer canvas (Published/Active)
2. `2-school-it-tickets-auto-classified.png` — Tickets auto-classified by the flow
3. `3-confirmation-emails-sent.png` — Email log with confirmation emails
4. `4-project-update-set-complete.png` — Update set in Complete state (35 updates)

## Team

- Amirthavarshini J (Team Lead)
- Ashwanth S
- Deepan M
- Aaditya Srinivasan K S

*Instance: nowlearning-nlinst04741515-242fm (ServiceNow Zurich, Personal Developer Instance)*
