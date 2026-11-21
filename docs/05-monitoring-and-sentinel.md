# 05 — Monitoring Purview with Microsoft Sentinel

## Goal

Understand how data-protection events can support SOC investigations, including label changes/removals and DLP policy matches.

> **Experience boundary:** I explored Purview monitoring, its Sentinel Content Hub integration and KQL-based alerts through training demonstrations. This section is an **illustrative monitoring reference**, not evidence of a production Sentinel deployment or graded lab implementation.

## Purview monitoring

- **Content Explorer:** Which detected sensitive data exists and where it is located (subject to access permissions).
- **Activity Explorer:** What supported classification, labeling and DLP activities occurred.
- **Audit:** Recorded supported activities for authorized investigation.
- **DLP alerts:** Review policy matches and user actions; an alert is not proof of successful exfiltration.

## Sentinel integration concept

Purview audit/classification event → supported data connector in Sentinel → table with labeling events → KQL hunt → optional scheduled analytics rule → SOC triage.

The data connector, table schema, operation strings, event coverage and permissions must be verified in each tenant.

### Query: label-removal events

The training demonstration filtered the `MicrosoftPurviewInformationProtection` table by `LabelEventType` and `OldSensitivityLabelId`. To avoid exposing a real tenant-specific identifier, this version uses a placeholder:

```kusto
MicrosoftPurviewInformationProtection
| where TimeGenerated >= ago(1h)
| where LabelEventType == "LabelRemoved"
| where OldSensitivityLabelId == "<YOUR_HIGHLY_CONFIDENTIAL_LABEL_GUID>"
| order by TimeGenerated desc
```

Replace the placeholder with **your own tenant's** label ID and validate that the columns exist. This detects **removal of that label**, not all downgrades. A change from Highly Confidential to Confidential may be represented by another event type.

### Query: inspect event types before refining a detection

```kusto
MicrosoftPurviewInformationProtection
| where TimeGenerated >= ago(7d)
| summarize Events = count() by LabelEventType
| order by Events desc
```

This is an exploratory query, not a universal schema guarantee.

### Example detection logic for analyst review

- Flag removal of the most restrictive label.
- Correlate the same user/file with subsequent external sharing.
- Investigate bursts of label removals or changes.
- Verify legitimate justification and whether the underlying data remained protected.

A scheduled analytics rule can create alerts based on query results. For production use, add an appropriate query period, frequency, deduplication strategy, incident grouping and validated entity mapping.

## Triage checklist

- Who changed or removed the label?
- Which file and original classification were involved?
- Was it a removal, a downgrade or another modification?
- Was business justification provided?
- Was there later external sharing or another suspicious action?
- Was disclosure prevented, allowed, or not established?

**Principle:** Sensitive-label changes deserve investigation in context, but aren't automatically malicious.

References: [Microsoft Sentinel](https://learn.microsoft.com/en-us/azure/sentinel/) · [Microsoft Purview](https://learn.microsoft.com/en-us/purview/).
