# Project 6 – Evidence Register

## 1. Purpose

This directory contains the screenshots collected during the implementation and validation of Project 6: Azure Identity and Access Management.

The evidence documents Microsoft Entra ID identities and group membership, the Azure RBAC configuration workflow and resulting access views, subscription portal views, sign-in records, and selected Azure CLI authorization tests.

## 2. Evidence Register

| Evidence ID | Evidence Description | What It Demonstrates |
|---|---|---|
| P6-E01 | Microsoft Entra security groups listing | The cloud security analysts security group is present. |
| P6-E02 | Microsoft Entra users listing | The dedicated analyst account is present. |
| P6-E03 | Analyst group membership | The `hti-p6-analyst` account is a member of `HTI-P6-Cloud-Security-Analysts`. |
| P6-E04 | Azure RBAC role-assignment wizard | Reader is selected as the role and the security group is selected as the member in the assignment workflow. This is the wizard/configuration stage, not standalone proof of a completed assignment. |
| P6-E05 | Azure subscription overview | The Azure subscription overview page. |
| P6-E06 | Analyst's Azure role assignments | The analyst's role assignments view displays the subscription-level Reader assignment. |
| P6-E07 | Subscription My permissions | The My permissions view for the analyst at subscription scope. |
| P6-E08 | Azure subscription Activity Log | The subscription Activity Log view. |
| P6-E09 | Subscription Access control (IAM) controls | In the analyst's captured IAM view, both **Add role assignment** and **Add custom role** are disabled. This shows the analyst cannot initiate those role-management actions through the portal in that session. |
| P6-E10 | Azure Resource groups view | The Resource groups page displays no resource groups at the time of capture. |
| P6-E11 | Microsoft Entra sign-in logs | Sign-in log entries for the analyst. |
| P6-E12 | Selected sign-in record details | The details view for a selected analyst sign-in record. |
| P6-E13 | Azure CLI identity and subscription context | Output from `az account show`, displaying the subscription and analyst identity context. |
| P6-E14 | Azure CLI resource-group creation attempt | The attempted resource-group creation returns `AuthorizationFailed` for the required `resourceGroups/write` permission. |
| P6-E15 | Azure CLI resource-group listing | `az group list --output table` completes and returns no resource groups. |

## 3. Key Validation Evidence

Three Azure CLI tests provide direct evidence of the analyst's identity context and the outcomes of the selected read and write operations.

### Identity and subscription context

```bash
az account show --query "{subscription:name, id:id, user:user.name}" -o table
```

The captured output displays the active Azure subscription and the signed-in analyst identity context.

### Read operation

```bash
az group list --output table
```

The command completes and returns an empty list because no resource groups were present in the subscription at the time of capture. An empty result is not a permission-denied response.

### Write operation

```bash
az group create --name rg-reader-permission-test --location eastus
```

The captured command output returns `AuthorizationFailed` and identifies the required `Microsoft.Resources/subscriptions/resourceGroups/write` permission. This demonstrates denial of this specific resource-group creation attempt.

## 4. Interpreting the IAM Screenshot

P6-E09 captures the analyst's Azure subscription **Access control (IAM)** page. In the opened **Add** menu, both **Add role assignment (disabled)** and **Add custom role (disabled)** are visible.

This supports the specific conclusion that those role-management options were unavailable to the analyst through the portal in the captured session. It should not be generalized into proof about every role-management operation through other interfaces or APIs.

## 5. Evidence Handling and Redaction

Before publishing screenshots:

- Remove or obscure personal email addresses where appropriate.
- Redact subscription IDs and tenant identifiers where appropriate.
- Never publish passwords, access tokens, client secrets, or recovery codes.
- Preserve relevant role names, permission details, command output, and error messages.
- Do not alter evidence in a way that changes the meaning of the result.

## 6. Important Notes

- P6-E04 records the selected role and group in the assignment wizard; P6-E06 is the subsequent role-assignment view.
- P6-E09 shows disabled role-management options in the captured IAM portal view.
- P6-E10 and P6-E15 show an empty resource-group inventory; they do not indicate that listing was denied.
- P6-E14 is the evidence of the tested resource-group creation denial.
- The sign-in evidence relates to the records shown and does not establish tenant-wide MFA or Conditional Access enforcement.
- The screenshots document the actual lab implementation and should not be represented as production deployment evidence.

## 7. Conclusion

The evidence documents the analyst's Entra ID group membership, the group-based Reader assignment workflow and resulting role-assignment view, the disabled role-management options visible in the analyst's subscription IAM portal view, and the outcomes of the selected Azure CLI tests.

Together, these screenshots support a bounded, evidence-based account of the Project 6 implementation without claiming that untested permissions or interfaces were validated.
