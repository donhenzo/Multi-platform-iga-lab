# Multi-Platform IGA Lab — Henz Industries

Identity governance lab using MidPoint as the core, provisioning to Okta and (later) Entra ID and AWS IAM Identity Center. Mock company: Henz Industries, 200 users, 8 departments, CSV as the HR source.

## Architecture

```
CSV (henz_hr.csv)
     |
     v
 MidPoint  ← roles, JML, policies
     |
     +--> Okta
     +--> Entra ID (pending)
            |
            v
    AWS IAM Identity Center (pending, via SCIM)
```

## Stack

- MidPoint 4.9 — Docker, Mac Intel
- Okta — developer trial tenant
- Entra ID — existing tenant (not yet connected)
- AWS IAM Identity Center — target via Okta SCIM (not yet connected)
- `henz_hr.csv` — 200-row mock HR data, 8 departments

## Done

- CSV resource → MidPoint, all 200 users imported, object template auto-assigns department Business Roles.
- Custom Okta connector built from source, resource live.
- Role model: department Business Role → job-title-based Application Role → Okta account + group.
- 6 Application Roles (Systems Admin, IAM Engineer, Cloud Engineer, Security Analyst, DevOps Engineer, Support), each provisions an Okta account into the matching `AWS-IT-*` group.
- Pilot of 6 users provisioned end-to-end in Okta, correct groups confirmed.

## Not done yet

- Okta → AWS IAM Identity Center (SCIM)
- Entra ID connector
- Access certification / SoD policy
- Live HR source instead of static CSV

## Structure

```
docs/troubleshooting-log.md   # errors hit, root cause, fix
midpoint/resources/           # resource connector XML (CSV, Okta)
midpoint/object-templates/    # object template
midpoint/roles/               # role definitions
data/henz_hr.csv
```

No live credentials in this repo. Okta group IDs are hardcoded per-tenant in the role XMLs, don for simplification, not meant to survive a move to a different Okta org.