# Project 6: Azure Identity and Access Management – Group-Based RBAC and Least Privilege

**Author:** Taiwo H Ibrahim  
**Platform:** Microsoft Azure  
**Identity service:** Microsoft Entra ID  
**Access control:** Azure Role-Based Access Control (RBAC)  
**Project type:** Practical Cloud Security Lab

## 1. Project Overview

This project demonstrates the implementation and validation of identity and access management controls in Microsoft Azure.

The primary objective was to establish a group-based access model using Microsoft Entra ID and Azure RBAC, ensuring that a designated cloud security analyst receives only the permissions required to perform read-only activities within an Azure subscription.

The project focuses on least privilege, centralized permission management, identity separation, and practical authorization testing.

## 2. Objectives

- Create and configure a Microsoft Entra ID security group.
- Create a dedicated analyst user account.
- Implement group-based Azure RBAC.
- Assign the built-in Reader role at subscription scope.
- Validate inherited permissions through group membership.
- Examine identity authentication and sign-in activity.
- Test permitted read operations and denied write operations.
- Document security observations and supporting evidence.

## 3. Lab Architecture

The lab uses a single Microsoft Entra tenant and one Azure subscription.

### Identity configuration

| Component | Configuration |
|---|---|
| Tenant | Default Directory |
| Subscription | Azure subscription 1 |
| Analyst user | hti-p6-analyst |
| User type | Member |
| Security group | HTI-P6-Cloud-Security-Analysts |
| Group membership | Assigned |
| Azure RBAC role | Reader |
| Assignment scope | Subscription |

### Access flow

1. The analyst authenticates to Microsoft Azure.
2. Microsoft Entra ID identifies the user and its group membership.
3. Azure RBAC evaluates the group's role assignment.
4. The Reader role grants permitted read operations.
5. Write operations outside the assigned permissions are denied.

## 4. Implementation

### Microsoft Entra ID

A dedicated security group named `HTI-P6-Cloud-Security-Analysts` was created.

A separate user account, `hti-p6-analyst`, was subsequently created and added to the group.

The analyst account was configured as a Member account and was not assigned an Entra directory administrator role.

### Azure RBAC

The built-in Reader role was assigned to the security group at subscription scope.

This approach centralizes access management: users receive the group's permissions through membership rather than requiring individual role assignments.

## 5. Security Validation

Testing was performed using the dedicated analyst account.

| Test | Operation | Expected outcome | Observed result |
|---|---|---|---|
| Authentication | Sign in to Azure Portal | Successful authentication | Successful |
| Subscription access | View subscription information | Allowed | Successful |
| Resource enumeration | `az group list` | Allowed | Successful; empty result |
| Resource-group creation | `az group create` | Denied | AuthorizationFailed |
| Role administration | Add role assignment | Denied | Portal controls disabled |
| Effective permissions | Inspect My permissions | Reader inherited through group | Confirmed |

### Command-line validation

**Read operation:**

```bash
az group list --output table
```

Result: Command completed successfully and returned an empty list because no resource groups existed in the subscription.

**Write operation:**

```bash
az group create --name rg-reader-permission-test --location eastus
```

Result: Azure returned `AuthorizationFailed` for:

```text
Microsoft.Resources/subscriptions/resourceGroups/write
```

No resource group was created.

These tests demonstrate that the analyst could perform the tested read operation while being denied the tested resource-creation operation.

## 6. Authentication Investigation

The analyst's Microsoft Entra sign-in records were examined.

The selected successful Azure Portal sign-in showed:

- Authentication status: Success.
- Authentication requirement: Single-factor authentication.
- Additional detail: MFA requirement satisfied by claim in the token.
- Conditional Access: Security defaults not applied to this sign-in.

The evidence does not establish that MFA was prompted during this particular sign-in or that a Conditional Access policy enforced MFA.

## 7. Security Observations

The project demonstrated:

- Centralized access management through Microsoft Entra security groups.
- Subscription-level least-privilege authorization using Azure RBAC.
- Separation of the administrator identity from the analyst identity.
- Visibility into subscription Activity Logs and role assignments.
- Successful read-only access validation.
- Enforcement of write restrictions through Azure authorization.

The subscription had no resource groups or deployed resources during the validation exercise. Consequently, resource inspection against an existing deployed workload was outside the scope of this implementation.

## 8. Limitations

- The implementation used a single Microsoft Entra tenant.
- Cross-tenant B2B collaboration was not implemented.
- No production workloads were deployed.
- The resource-creation denial was validated, but other write operations were not exhaustively tested.
- The authentication investigation did not establish that Conditional Access enforced MFA.
- Defender for Cloud displayed no recommendations in the inspected views; this was not a comprehensive security assessment.

## 9. Evidence

Supporting screenshots document:

- Security group creation.
- Analyst account creation and membership.
- Group-based Reader assignment.
- Successful analyst authentication.
- Subscription and Activity Log visibility.
- RBAC assignment visibility.
- Denied resource-group creation.
- Successful read-only CLI operation.
- Authentication and sign-in investigation.

Sensitive subscription and account information will be redacted before public publication.

## 10. Conclusion

This project provided practical experience implementing and validating identity-based access control in Microsoft Azure.

It demonstrated how Microsoft Entra ID security groups and Azure RBAC can be combined to provide centralized, least-privilege access to cloud resources.

The most important validation was the successful execution of a permitted read operation alongside an explicitly denied resource-creation operation.

The project forms part of an ongoing cybersecurity portfolio focused on cloud security, IAM hardening, SOC operations, and defensive security.

---

**GitHub Portfolio Author:** Taiwo H Ibrahim
