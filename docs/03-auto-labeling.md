# 03 — Automatic sensitivity labeling

## Goal

Automatically assign a data classification when content meets the configured detection conditions.

## Assessment scenario

Configure a policy to apply **Private → Internal** to Microsoft Excel files in **SharePoint Online and OneDrive** that contain **one or more credit card numbers**.

| Setting | Configured target |
| --- | --- |
| Locations | SharePoint Online and OneDrive |
| Sensitive information type | Credit Card Number |
| Minimum instances | 1 |
| File extension | `.xlsx` |
| Target sensitivity label | Private / Internal |
| Rule logic | Credit Card Number match **AND** `.xlsx` extension |

This is **service-side** auto-labeling: Purview evaluates content in supported cloud locations. Client-side auto-labeling instead evaluates documents/emails in supported Office applications as people work.

## Workflow I followed

1. Open Information Protection → Auto-labeling policies.
2. Create a custom policy, choose SharePoint and OneDrive locations.
3. Use a common rule because the same conditions apply to both locations.
4. Select Credit Card Number SIT with minimum instance count of 1.
5. Add the Excel file-extension condition.
6. Select the **Private / Internal** sensitivity label.
7. Review and create the policy, then inspect its **simulation** status.

## Troubleshooting and observations

- An incompatible label encryption configuration caused the error: **the selected Internal label can only be used with Exchange location**. I checked the label's access-control configuration rather than removing the required SharePoint/OneDrive locations.
- A duplicate policy-name error appeared when trying to create the policy again. I returned to the existing policy to review/edit it instead of producing duplicates.
- The policy initially appeared in **Simulation** mode; activation is separate from policy creation and can depend on processing/eligibility.

**Evidence boundary:** This document records policy configuration and observed simulation screens. It does **not** claim that the policy finished scanning production content or that full enforcement was verified.

## Key lesson

An SIT detects relevant data; an auto-labeling policy applies a classification under configured conditions; a sensitivity label may then enforce compatible protection. **Detecting** a credit card number alone does not automatically apply a label.

Reference: [Apply sensitivity labels automatically](https://learn.microsoft.com/en-us/purview/apply-sensitivity-label-automatically).
