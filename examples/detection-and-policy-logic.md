# Example rules and validation notes

These examples explain the logic behind the assessment configurations. They are **documentation/pseudocode**, not exported Purview policy files, deployable scripts or proof of an active production environment.

## Custom SIT example

```yaml
name: SIT1
primary_element:
  type: keyword_dictionary
  name: Dictionary1
  terms: [Contoso, Litware, Adventureworks]
supporting_element: Func_ssn
character_proximity: 200
pattern_confidence: medium
recommended_confidence: medium
```

## Auto-labeling logic

```text
Locations: SharePoint Online and OneDrive
IF Content contains Credit Card Number SIT (minimum 1 occurrence)
AND File extension is xlsx
THEN Apply sensitivity label Private / Internal
```

The SIT match and extension condition both matter. The actual service-side policy can begin in simulation and may need a separate activation step.

## DLP logic

```text
IF Content matches General Password SIT
AND applicable sharing/location conditions are satisfied
THEN apply configured Microsoft 365 restrictions,
     display user policy tips,
     and generate high-severity administrator alerts
DEPLOY in simulation/test mode with policy tips for initial validation
```

The assessment tested configuration in a temporary tenant. **Simulation mode does not enforce blocking**.

## Validation checklist

- [ ] Check exact condition/operator and instance count
- [ ] Check selected workload locations
- [ ] Confirm target label scope and encryption compatibility
- [ ] Use synthetic test data without real credentials or customer records
- [ ] Review simulation results and false positives
- [ ] Validate policy tip and alert severity
- [ ] Verify actual rule operation and remediation before enabling enforcement
