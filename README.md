# Microsoft Purview — Information Protection & Data Loss Prevention Lab

Hands-on Microsoft Purview learning and a **Microsoft Applied Skills assessment** covering data discovery, sensitivity labels, automatic labeling, DLP policies, and security monitoring concepts with Microsoft Sentinel.

> **Scope and honesty:** The configurations in this repository come from a temporary Microsoft-provided assessment lab completed on **21 November 2026** and related learning exercises. This is a **training portfolio**, not a production deployment or an independently operated enterprise tenant. Sentinel integration and KQL monitoring were explored in learning demonstrations; they are not represented as part of the graded Purview assessment.

## Why I worked on this

My earlier security work focused mainly on SOC alert investigation and incident response. I wanted to understand the controls that identify and protect sensitive information **before it becomes a data-security incident**, and how the resulting activities can be monitored by a SOC.

## What I practiced

| Area | Hands-on work / learning |
| --- | --- |
| Sensitive Information Types (SITs) | Created a custom SIT using a keyword dictionary, supporting SSN detection, proximity and confidence settings |
| Information protection | Created a **Private** label group and **Internal / External** sublabels; worked with scopes, content marking and encryption permissions |
| Label publishing | Published labels with a policy requiring justification for label removal or downgrading |
| Automatic labeling | Configured a policy to label qualifying Excel files in SharePoint Online and OneDrive using the Credit Card Number SIT |
| Data Loss Prevention | Configured a credential-protection DLP rule with policy tips, notifications, high-severity alerts, and simulation mode |
| Monitoring | Explored Purview activity/audit capabilities, Microsoft Sentinel integration via Content Hub and KQL-based label-change detections |

## Walkthrough

1. [Sensitive Information Types and detection methods](docs/01-sensitive-information-types.md)
2. [Sensitivity labels, permissions and publishing](docs/02-sensitivity-labels.md)
3. [Auto-labeling SharePoint and OneDrive files](docs/03-auto-labeling.md)
4. [Data Loss Prevention policy and alert workflow](docs/04-data-loss-prevention.md)
5. [Monitoring and Microsoft Sentinel / KQL](docs/05-monitoring-and-sentinel.md)

## Tools and concepts

Microsoft Purview · Microsoft 365 · SharePoint Online · OneDrive · Exchange Online · Microsoft Teams · SITs · keyword dictionaries · sensitivity labels · encryption · DLP · audit · Microsoft Sentinel · KQL

## Example detection lifecycle

```text
Document / email
    |
    v
Sensitive information detection (SIT / classifier)
    |
    +--> Auto-labeling policy (if configured)
    |       |
    |       v
    |   Sensitivity label and optional protection
    |
    +--> DLP policy evaluates data and activity
            |
            +--> allow / warn / restrict (configured action)
            +--> audit / alert / investigate
```

The label and DLP paths are **not dependent on each other**: DLP can match a SIT directly without a sensitivity label, and a label can be applied without an automatic classifier.

## Credential

**Microsoft Applied Skills — Implement information protection and data loss prevention by using Microsoft Purview** (passed 21 November 2026).

The certificate image/PDF is **not yet uploaded**. A placeholder directory is available in [certificates/](certificates/) for adding it later.

## Privacy and reproducibility

This portfolio uses explanatory examples and **does not contain assessment login credentials, tenant IDs, private user data, or exported configurations**. Microsoft assessment scenarios may use illustrative company names and sample patterns. Always test DLP policies in simulation before enforcing them in a real tenant. Available settings depend on licensing, permissions, workload and current product functionality.

## Useful references

- [Microsoft Purview documentation](https://learn.microsoft.com/en-us/purview/)
- [Sensitive information types](https://learn.microsoft.com/en-us/purview/sensitive-information-type-learn-about)
- [Sensitivity labels](https://learn.microsoft.com/en-us/purview/sensitivity-labels)
- [DLP overview](https://learn.microsoft.com/en-us/purview/dlp-learn-about-dlp)
- [Microsoft Sentinel documentation](https://learn.microsoft.com/en-us/azure/sentinel/)
