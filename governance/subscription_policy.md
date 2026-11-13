# Brightline Logistics - SaaS Subscription Governance Policy

Applies to the monthly SaaS seat utilization audit. The authoritative monthly
export is `data/subscription_usage_export.csv` and the application registry is `governance/app_registry.json`.

## 1. Definitions

For every application in the export:

- `utilization_pct` = round(seats_active_30d / seats_purchased * 100, 1)
- `seat_unit_price` = round(monthly_cost_usd / seats_purchased, 2)
- `monthly_waste_usd` = round((seats_purchased - seats_active_30d) * seat_unit_price, 2)

## 2. Decision matrix

Evaluate each application against the registry attributes (`auto_renew`,
`criticality`) in this priority order:

1. `RECLAIM` - `utilization_pct` < 50.0 AND `auto_renew` is true AND
   `monthly_waste_usd` >= 1500.00
2. `DOWNSIZE` - `utilization_pct` < 65.0 AND `monthly_waste_usd` >= 500.00
3. `REVIEW` - `utilization_pct` < 75.0
4. `RETAIN` - otherwise

**Tier-1 exception:** any application whose registry `criticality` is `tier1`
must never be labelled `RECLAIM` or `DOWNSIZE`; if the matrix above would
produce either of those, the final decision is downgraded to `REVIEW`.

## 3. Escalation

An application is escalated when its final decision is `RECLAIM` and its
`monthly_waste_usd` >= 2000.00. For every escalated
application open one issue in this repository with:

- Title exactly: `[Seat Reclaim] <app_id> - <app_name>`
- Body containing these four lines:
  - `Application: <app_id>`
  - `Owner: <owner_handle from the registry>`
  - `Monthly waste: $<monthly_waste_usd with exactly 2 decimals>`
  - `Utilization: <utilization_pct>%`

## 4. Audit summary

Commit the audit summary to `reports/seat_utilization_audit.md`. It must contain these
labelled lines (one per line, exact labels):

```
Total applications: <count>
RECLAIM: <count>
DOWNSIZE: <count>
REVIEW: <count>
RETAIN: <count>
Total monthly waste (RECLAIM+DOWNSIZE): $<amount with exactly 2 decimals>
Escalated applications: <comma-separated app_id list, ascending order>
```

The summary must account for every application in the export exactly once.
