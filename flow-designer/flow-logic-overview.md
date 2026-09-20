# SoftReq — Flow Logic Overview

Flow Designer flows are stored as platform configuration, not as
readable source code, so this document describes the flow's logic
in plain language for anyone reviewing this repository.

**Flow name:** Software Request Approval Flow
**Trigger table:** Request [sc_request]
**Trigger type:** Record Created

## Steps

1. **Trigger — Record Created**
   Fires when a new Request record is created on the `sc_request`
   table (generated automatically when someone orders the
   "New Software License Request" catalog item).

2. **Action — Ask For Approval**
   - Table: `sc_request`
   - Record: the triggering Request record
   - Approver: dynamically resolved as
     `Trigger Record → Requested for → Manager`
     (the manager of whoever the request was made for — not a
     fixed, hardcoded user)
   - Approval rule: Anyone approves (single approver)

3. **Flow Logic — If / Else**
   Branches based on `Ask For Approval → Approval State`:
   - **If Approved** → goes to step 4a
   - **Else (Rejected)** → goes to step 4b

4a. **Action — Update Record (Approved branch)**
   - Table: `sc_request`
   - Field updated: `State` → `Closed Complete`

4b. **Action — Update Record (Rejected branch)**
   - Table: `sc_request`
   - Field updated: `State` → `Closed Rejected`

5. **Action — Send Notification (both branches)**
   - Table: `sc_request`
   - Notification used: `Request Status Update`
   - Recipient: `Requested for`'s email
   - Message includes the request `Number` and current `State`,
     so the requester is notified automatically whether their
     request was approved or rejected — no manual follow-up
     needed from IT.

## Why this design

- **Dynamic approver (Manager reference)** instead of a hardcoded
  user — the flow adapts to whoever submitted the request, which
  is how a real multi-employee approval process would need to work.
- **Flow Designer over the legacy Workflow editor** — Flow Designer
  is ServiceNow's current, actively maintained automation tool and
  handles approval routing, branching, and record updates natively
  without custom server-side scripting.
- **Single notification with dynamic content** (`${state}`) instead
  of two separate notification records — simpler to maintain, one
  source of truth for the message template.
