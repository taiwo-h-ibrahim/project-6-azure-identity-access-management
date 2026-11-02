# Project 6 – Evidence Register

## 1. Purpose

This directory contains screenshots and supporting evidence collected during the implementation and validation of Project 6: Azure Identity and Access Management.

The evidence demonstrates the configuration of Microsoft Entra ID identities, group-based Azure RBAC, effective permissions, and successful and denied access operations.

## 2. Evidence Register

| Evidence ID | Evidence Description | What It Demonstrates |
|---|---|---|
| P6-E01 | Security group creation | Creation of `HTI-P6-Cloud-Security-Analysts` |
| P6-E02 | Analyst user account | Creation and configuration of the analyst account |
| P6-E03 | Group membership | Analyst account added to the security group |
| P6-E04 | Azure RBAC assignment | Reader role assigned to the security group |
| P6-E05 | Analyst subscription overview | Analyst can access the Azure subscription |
| P6-E06 | Analyst role assignments | Reader role inherited through group membership |
| P6-E07 | My permissions | Effective Reader permissions at subscription scope |
| P6-E08 | Activity Log | Visibility of subscription activity |
| P6-E09 | IAM role assignment controls | Analyst cannot add role assignments through the portal |
| P6-E10 | Resource Groups portal view | Analyst's access to the Resource Groups area |
| P6-E11 | Authentication details | Successful analyst sign-in and authentication details |
| P6-E12 | Conditional Access details | Conditional Access status shown for the selected sign-in |
| P6-E13 | Azure Cloud Shell identity validation | `az account show` confirms the analyst identity and subscription context |
| P6-E14 | Resource-group creation attempt | Azure CLI returns `AuthorizationFailed` for the attempted write operation |
| P6-E15 | Resource-group listing | `az group list` executes successfully and returns an empty result |

## 3. Key Validation Evidence

Three Azure CLI tests provide direct evidence of the analyst's access:

### Identity and subscription context

```bash
az account show --query "{subscription:name, id:id, user:user.name}" -o table
```

Confirms the active Azure subscription and signed-in analyst identity.

### Read operation

```bash
az group list --output table
```

The command succeeds and returns an empty list because no resource groups existed in the subscription.

### Write operation

```bash
az group create --name rg-reader-permission-test --location eastus
```

The command returns `AuthorizationFailed`, demonstrating that the analyst does not have the required resource-group creation permission.

## 4. Evidence Handling and Redaction

Before publishing screenshots:

- Remove or obscure personal email addresses where appropriate.
- Redact subscription IDs and tenant identifiers.
- Never publish passwords, access tokens, client secrets, or recovery codes.
- Preserve relevant role names, permission details, command output, and error messages.
- Do not alter evidence in a way that changes the meaning of the result.

## 5. Important Notes

- The resource-group listing returned no entries; this does not mean the command was denied.
- The resource-group creation attempt was denied by Azure authorization.
- The authentication evidence relates to the specific sign-in examined and does not establish tenant-wide MFA enforcement.
- The Defender for Cloud portal observations are documented separately and should not be interpreted as proof that every resource or workload was scanned.
- The screenshots document the actual lab implementation and should not be represented as production deployment evidence.

## 6. Conclusion

The evidence supports the implementation of group-based Azure RBAC and least-privilege access in a single-tenant environment.

The combination of portal observations and Azure CLI tests demonstrates that the analyst can perform permitted read operations while being denied the tested resource-creation operation.
