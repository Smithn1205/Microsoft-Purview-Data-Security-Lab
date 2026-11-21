# 02 — Sensitivity labels and label policies

## Goal

Classify content and configure appropriate information protection in supported Microsoft 365 locations.

## Assessment configuration

Created a label group named **Private**, with two sublabels:

```text
Private
├── Internal
└── External
```

The assessment required **Private** to have highest priority.

### Internal sublabel

| Setting | Configuration |
| --- | --- |
| Scope | Items → **Files and Emails** |
| Content marking | **Header** |
| Header text | `Internal private` |
| Alignment | Center |

The task did not require a 14-day expiry for Internal. **Do not infer that Internal has External's encryption settings.** This distinction was important when troubleshooting the auto-labeling policy.

### External sublabel

| Setting | Configuration |
| --- | --- |
| Scope | Items → **Files and Emails** |
| Encryption | Apply protection with administrator-assigned permissions |
| Allowed users | All authenticated users (as specified in exercise) |
| Content expiration | **14 days** after label application |
| Offline access | Never |

The 14-day duration governs protected content access, **not document deletion**. The exact available controls depend on the Microsoft 365 tenant and client.

## Publishing label policies

A sensitivity label is created first, then **published** through a label policy to the intended users/groups. The training exercise required publishing Private and its sublabels and enabling **justification for label removal or a downgrade**.

### Know the difference

| Component | Responsibility |
| --- | --- |
| Scope | Where a label is eligible to be used (files, emails, meetings, sites, etc.) |
| Sensitivity label | Classification and optional encryption/content markings |
| Label priority | Ordering relevant to labeling/downgrading behavior |
| Label policy | Which users see labels, mandatory/default behavior and justification |
| DLP policy | Actions and restrictions when sensitive content or behaviors match conditions |

A label named **Public** does not, by itself, expose a file publicly. Actual file access remains governed by storage permissions and any configured label-based encryption.

## Troubleshooting lesson

While setting up server-side auto-labeling for SharePoint/OneDrive, the **Internal** label initially used **Let users assign permissions when they apply the label**. Purview reported that this label was only usable with Exchange for that auto-labeling scenario. The lesson: verify whether a target label is compatible with the specific auto-labeling workload. Do not weaken required protection simply to dismiss a validation message.

Reference: [Sensitivity labels](https://learn.microsoft.com/en-us/purview/sensitivity-labels).
