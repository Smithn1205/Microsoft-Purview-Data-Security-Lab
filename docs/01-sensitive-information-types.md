# 01 — Detecting sensitive information

## Goal

Understand how Microsoft Purview **detects** information before a sensitivity label or DLP rule takes action.

## What I configured in the assessment

I created a custom SIT with these example settings:

| Setting | Configuration |
| --- | --- |
| SIT name | `SIT1` |
| Primary element | Keyword dictionary |
| Dictionary name | `Dictionary1` |
| Entries | `Contoso`, `Litware`, `Adventureworks` |
| Supporting element | `Func_ssn` |
| Character proximity | `200` |
| Pattern confidence | Medium |
| Recommended confidence | Medium |

**Detection meaning:** A dictionary keyword is the primary match and the SSN function is supporting evidence within the configured proximity. Confidence represents the evidence required for the SIT match; it is **not** a sensitivity classification such as Confidential.

**Practical workflow:** Information Protection → Classifiers → Sensitive info types → Create → Define primary and supporting elements → Set confidence/proximity → Review and test.

The example company terms are sample lab values, not a private company's secret identifiers.

## Detection methods studied

| Method | What it matches | Useful example |
| --- | --- | --- |
| Built-in SIT | Microsoft-defined patterns/validators | Credit card numbers, passports |
| Custom SIT | Administrator-defined patterns, functions, keywords | Employee identifier format |
| Exact Data Match (EDM) | Known values from securely prepared reference data | Actual customer account numbers |
| Document fingerprinting | Content resembling a known document template | Standard contract or form |
| Named entity SIT | Names, physical addresses, medical terminology | Patient information |
| Trainable classifiers | Document categories/content characteristics | Resume or contract |

### EDM vs. ordinary SITs

A regex might detect **any** identifier matching a format. EDM is intended to find **known records** from a reference table. The organization defines a schema, securely hashes and uploads source data, and Purview matches candidate values using its EDM process. EDM is not a conventional live plaintext database query and is not identical to password authentication hashing.

### Document fingerprinting

A fingerprint is based on recognizable **text/content** from a standard template, not a byte-for-byte hash of the whole Word or PDF file. It can help identify completed variants of a form.

### Keyword dictionaries and named entities

Keyword dictionaries collect organization-specific phrases. Named-entity SITs can detect categories such as full names, addresses and medical terms; bundled types cover broad groups, while unbundled types are narrower.

## Important distinction

- **SIT/classifier:** What information was detected?
- **Sensitivity label:** How is the content classified and optionally protected?
- **Auto-labeling policy:** Should matching content receive a label?
- **DLP policy:** What should happen when sensitive content is used or shared?

A SIT match on its own does **not** apply a label or block sharing.

## Validation and limitations

Before enforcing DLP, test for false positives/negatives using safe synthetic data. Pattern accuracy depends on the selected elements, supporting context, proximity and confidence. Some Purview workloads require separate licensing or configuration.

Reference: [Microsoft SIT documentation](https://learn.microsoft.com/en-us/purview/sensitive-information-type-learn-about).
