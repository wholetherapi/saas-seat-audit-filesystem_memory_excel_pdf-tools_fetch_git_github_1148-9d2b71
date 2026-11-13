# Brightline Logistics - SaaS Governance Workspace

This repository holds the monthly SaaS subscription usage export together with
the governance policy and application registry used by the IT asset team.

## Layout

- `data/subscription_usage_export.csv` - authoritative monthly usage export. It lists exactly
  **320** tracked applications and the last line of the file is
  `# END_OF_EXPORT`.
- `governance/app_registry.json` - application registry (criticality, auto-renew flag and
  owning team handle for each `app_id`).
- `governance/subscription_policy.md` - decision matrix, escalation rules and audit report
  requirements.
- `reports/seat_utilization_audit.md` - audit summary produced by the analyst.

The raw text of the export is served at:

`https://raw.githubusercontent.com/wholetherapi/saas-seat-audit-filesystem_memory_excel_pdf-tools_fetch_git_github_1148-9d2b71/main/data/subscription_usage_export.csv`
