# Project 6 – Azure IAM Architecture

## 1. Overview

This document describes the identity and access management architecture implemented in Project 6.

The lab demonstrates how Microsoft Entra ID security groups and Azure Role-Based Access Control (RBAC) can be combined to implement group-based access and least-privilege permissions within a single Azure tenant.

The implementation focuses on granting read-only access at the subscription scope while preventing an analyst account from creating or modifying resources.

## 2. Lab Architecture

The architecture consists of:

- **Microsoft Entra ID:** Provides identity and group management.
- **Security group:** `HTI-P6-Cloud-Security-Analysts`
- **Analyst account:** `hti-p6-analyst`
- **Azure subscription:** `Azure subscription 1`
- **Azure RBAC role:** Built-in Reader role.
- **Assignment scope:** Azure subscription.

## 3. Identity and Access Structure

| Component | Configuration |
|---|---|
| Directory | Default Microsoft Entra ID tenant |
| Analyst account | HTI P6 Lab Analyst |
| User type | Member |
| Security group | HTI-P6-Cloud-Security-Analysts |
| Membership type | Assigned |
| Group membership | Analyst account added |
| RBAC role | Reader |
| Assignment target | Security group |
| Assignment scope | Subscription |
| Directory administrative role | None assigned to analyst |

## 4. Access Flow

The access model follows this sequence:

1. The administrator creates the analyst account in Microsoft Entra ID.
2. The administrator creates the security group.
3. The analyst account is added to the security group.
4. The Azure Reader role is assigned to the security group at subscription scope.
5. The analyst signs in to Azure.
6. The analyst receives inherited Reader permissions through group membership.

The role is assigned to the group rather than directly to the individual user.

## 5. Architecture Diagram

```text
                 Microsoft Entra ID
                         |
              HTI-P6 Lab Analyst
                         |
                  Member of
                         |
                         v
       HTI-P6-Cloud-Security-Analysts
                         |
                 Azure RBAC Assignment
                         |
                    Reader Role
                         |
                         v
                 Azure Subscription
                         |
              Read-only permissions
                         |
             +-----------+-----------+
             |                       |
             v                       v
       Read operations         Write operations
          Allowed                 Denied
```

## 6. Permission Validation

The architecture was validated using the Azure portal and Azure Cloud Shell.

| Test | Expected result | Observed result |
|---|---|---|
| View subscription | Allowed | Successful |
| View role assignments | Allowed | Successful |
| List resource groups | Allowed | Successful; empty result |
| Create resource group | Denied | AuthorizationFailed |
| Add role assignment | Denied | Portal controls unavailable to analyst |

The successful resource-group listing returned no entries because no resource groups existed in the subscription.

The attempted resource-group creation failed because the analyst lacked the required `Microsoft.Resources/subscriptions/resourceGroups/write` permission.

## 7. Security Design Principles

This implementation demonstrates:

- **Least privilege:** The analyst receives Reader rather than Contributor or Owner.
- **Group-based access control:** Permissions are assigned to a security group instead of an individual account.
- **Separation of duties:** Administrative responsibilities remain with the administrator account.
- **Subscription-level RBAC:** Access is controlled at the subscription scope.
- **Permission validation:** Both permitted and denied operations were tested.

## 8. Scope and Limitations

This project implements single-tenant Azure identity and access management.

It does not implement:

- Cross-tenant B2B collaboration.
- Guest-user access from an external tenant.
- Custom Azure RBAC roles.
- Production workloads or deployed Azure resources.
- A Conditional Access policy enforcing MFA.

These are outside the scope of this lab and may be explored in subsequent projects.

## 9. Conclusion

The lab demonstrates a practical implementation of group-based Azure RBAC and least-privilege access.

The analyst account successfully accessed subscription information through inherited group permissions but was unable to perform a resource-creation operation.

This provides practical evidence of how Microsoft Entra ID and Azure RBAC can be used together to control access within an Azure environment.
