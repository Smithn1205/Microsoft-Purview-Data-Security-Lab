# 04 — Data Loss Prevention (DLP)

## Goal

Detect sensitive information being used or shared in risky ways and apply appropriate policy decisions, user warnings and analyst notifications.

## Assessment scenario

Created/configured a DLP policy to protect **user login credentials** in supported Microsoft 365 services.

| Setting | Lab requirement / learning |
| --- | --- |
| Content detection | Sensitive info type: **General Password** |
| Target locations | Exchange, Teams, SharePoint and OneDrive |
| User notification | Policy tip |
| Custom tip text | `Sharing user login credentials is not allowed.` |
| Administrator alerts | Enabled, high severity |
| Deployment mode | **Test / simulation with policy tips** |

The assessment included configuration of sharing restrictions while running the policy in **simulation**, so this is **not** a claim that live user traffic was actually blocked.

## Rules: conditions, exceptions, actions

- **Conditions** are the *IF*: e.g., content matches a General Password SIT, and (where supported) sharing context.
- **Actions** are the *THEN*: e.g., restrict external access, show tips, or generate alerts.
- **Exceptions** specify *unless* conditions that exclude a rule match.

A DLP rule can use **SITs** and/or **sensitivity labels**. DLP doesn't require a sensitivity label; it can trigger directly on the content classifier.

### Rule example (conceptual)

```text
IF sensitive content = General Password
AND sharing context matches configured policy
THEN show policy tip and report the match
WITH High alert severity
IN test mode (do not enforce blocking yet)
```

**What I learned while configuring:** Some choices like **Block only people outside my organization** require an applicable external-sharing condition, and condition availability can change by policy type and selected location. Check the actual supported rule editor rather than assuming every option is present.

## Alerts vs. incidents

A policy match can generate an alert that analysts inspect in Purview or the Defender security experience. An alert does **not** prove that data was exfiltrated; the security team should check the user, application, detected data, destination, action taken and whether sharing was actually blocked.

## Monitoring workflow

1. Review DLP alerts and Activity Explorer.
2. Identify the rule, sensitive information type, user and affected workload.
3. Verify action/impact: attempted versus successful disclosure.
4. Determine whether activity was authorized; escalate only when justified by evidence.

## Operational best practices

Use synthetic examples for testing. Start in simulation with policy tips, review false positives and scope, and only then consider enforcing blocks in an authorized environment.

Reference: [Microsoft Purview DLP](https://learn.microsoft.com/en-us/purview/dlp-learn-about-dlp).
