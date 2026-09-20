# SoftReq — IT Software Request & Approval Automation

A ServiceNow Service Catalog + Flow Designer project that lets employees
self-serve software license requests, with automatic manager approval
routing — removing the need to email IT and wait.

## Problem
Employees requesting software had no structured way to request it or
get manager sign-off, leading to manual email threads and no audit trail.

## Solution
- A Service Catalog item captures the software name, requester, and
  business justification (via a reusable Variable Set).
- A Catalog UI Policy makes justification mandatory once software is named.
- A Flow Designer flow automatically routes the request to the
  requester's manager, branches on approve/reject, updates the request
  stage, and notifies the requester by email.

## Built with
ServiceNow (Zurich release, PDI) — Service Catalog, Variable Sets,
Catalog UI Policy, Flow Designer, Notifications

## Screenshots


## Update Set
Exported update set available at /update-set for import into any
ServiceNow instance.
