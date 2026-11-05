# Project 6 – Evidence Register

## 1. Purpose

This directory contains the screenshots collected during the implementation and validation of Project 6: Azure Identity and Access Management.

The evidence is separated into the original **core Azure IAM exercise** and the **B2B External Guest RBAC extension** so that each exercise has its own evidence folder, README, and reviewable evidence sequence.

## 2. Evidence Structure

The evidence is organized into two independent exercise folders:

- [core-azure-rbac/README.md](core-azure-rbac/README.md) — original Project 6 core evidence, P6-E01 through P6-E15.
- [b2b-external-guest-rbac/README.md](b2b-external-guest-rbac/README.md) — B2B External Guest RBAC extension, P6-B2B-E01 through P6-B2B-E08.

The core evidence and B2B extension retain their original evidence IDs. The B2B extension does not replace or renumber the original evidence.

## 3. Core Azure RBAC Evidence

See the dedicated core evidence register:

[core-azure-rbac/README.md](core-azure-rbac/README.md)

The core exercise demonstrates the internal analyst's group membership, subscription-level Reader assignment, IAM portal controls, and selected CLI authorization results.

### Core evidence sequence

| Evidence ID | Evidence Description | What It Demonstrates |
|---|---|---|
| P6-E01 | Microsoft Entra security groups listing | The cloud security analysts security group is present. |
| P6-E02 | Microsoft Entra users listing | The dedicated analyst account is present. |
| P6-E03 | Analyst group membership | The analyst account is a member of the cloud security analysts group. |
| P6-E04 | Azure RBAC role-assignment wizard | Reader is selected as the role and the security group is selected as the member in the assignment workflow. |
| P6-E05 | Azure subscription overview | The Azure subscription overview page. |
| P6-E06 | Analyst's Azure role assignments | The analyst's role assignments view displays the subscription-level Reader assignment. |
| P6-E07 | Subscription My permissions | The My permissions view for the analyst at subscription scope. |
| P6-E08 | Azure subscription Activity Log | The subscription Activity Log view. |
| P6-E09 | Subscription Access control (IAM) controls | The captured IAM view shows Add role assignment and Add custom role disabled for the analyst session. |
| P6-E10 | Azure Resource groups view | The Resource groups page displays no resource groups at the time of capture. |
| P6-E11 | Microsoft Entra sign-in logs | Sign-in log entries for the analyst. |
| P6-E12 | Selected sign-in record details | The details view for a selected analyst sign-in record. |
| P6-E13 | Azure CLI identity and subscription context | Output from `az account show`, displaying the subscription and analyst identity context. |
| P6-E14 | Azure CLI resource-group creation attempt | The attempted resource-group creation returns `AuthorizationFailed` for the required `resourceGroups/write` permission. |
| P6-E15 | Azure CLI resource-group listing | `az group list --output table` completes and returns no resource groups. |

## 4. B2B External Guest RBAC Extension

The B2B extension is documented separately so its evidence can be reviewed as an independent exercise while remaining part of Project 6.

See the dedicated extension README:

[b2b-external-guest-rbac/README.md](b2b-external-guest-rbac/README.md)

### B2B evidence sequence

| Evidence ID | Evidence Description | What It Demonstrates |
|---|---|---|
| P6-B2B-E01 | External B2B guest invitation/user configuration | The external Gmail identity was configured as a guest in the resource tenant. |
| P6-B2B-E02 | Successful external guest invitation redemption | The B2B invitation redemption completed successfully. |
| P6-B2B-E03 | Guest RBAC assignment / Check access | Reader and Storage Blob Data Reader are assigned to the guest at the storage-account scope. |
| P6-B2B-E04 | Azure CLI authentication | The external Gmail guest authenticates to the resource tenant with `az login --tenant`. |
| P6-B2B-E05 | Authenticated guest identity and tenant context | `az account show` confirms the external Gmail identity and selected resource-tenant subscription context. |
| P6-B2B-E06 | Blob read succeeds | The guest successfully retrieves the test Blob using `--auth-mode login`. |
| P6-B2B-E07 | Blob write denied | The controlled Blob upload attempt is rejected by Azure authorization. |
| P6-B2B-E08 | Blob delete denied | The Blob delete attempt is rejected by Azure authorization. |

## 5. Key Validation Evidence

### Core exercise

The core evidence demonstrates the internal analyst's group membership, subscription-level Reader assignment, IAM portal controls, and selected CLI authorization results.

### B2B extension

The B2B evidence demonstrates the complete external-access path:

**External Gmail identity → B2B guest authentication → Azure OAuth/CLI context → resource-scoped RBAC → read allowed → write/delete denied**

The strongest validation is the combination of P6-B2B-E05 through P6-B2B-E08:

- the authenticated identity is confirmed;
- the Blob read succeeds;
- the Blob write is denied;
- the Blob delete is denied.

## 6. Evidence Handling and Redaction

Before publishing screenshots:

- Remove or obscure personal email addresses where appropriate.
- Redact subscription IDs and tenant identifiers where appropriate if they are not needed to demonstrate the result.
- Never publish passwords, access tokens, client secrets, or recovery codes.
- Preserve relevant role names, permission details, command output, and error messages.
- Do not alter evidence in a way that changes the meaning of the result.

## 7. Important Notes

- P6-E04 records the selected role and group in the assignment wizard; P6-E06 is the subsequent role-assignment view.
- P6-E09 shows disabled role-management options in the captured IAM portal view.
- P6-E10 and P6-E15 show an empty resource-group inventory; they do not indicate that listing was denied.
- P6-E14 is the evidence of the tested resource-group creation denial.
- The B2B extension does not replace or renumber the original evidence.
- The B2B portal observation is specific to the lab configuration and should not be presented as a universal limitation of Azure B2B guests.
- The screenshots document the actual lab implementation and should not be represented as production deployment evidence.

## 8. Conclusion

The Project 6 evidence set contains both the original single-tenant Azure IAM exercise and a separately documented B2B external guest RBAC extension.

Each exercise now has its own evidence folder and README, preserving the original work while making the extension evidence easy for a reviewer to identify and evaluate.
